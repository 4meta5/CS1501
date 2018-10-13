# Coding Rust Cheatsheet (2 of 2)

Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it.

These are notes from [The Rust Book](https://doc.rust-lang.org/book/), but they also may draw from [Steve Donovan's Gentle Intro](https://stevedonovan.github.io/rust-gentle-intro/readme.html). **These notes only cover the last 7 chapters of The Book** 

* [Closure](#closure)
* [Iterator](#iterator)
    * [Loops vs. Iterators](#loopsvsiterators)
* [Cargo and Crates](#cargo)
    * [Cargo Workspaces](#workspace)
* [Smart Pointers](#smartpointers)

## Closures: Anonymous Functions that Capture the Environment <a name="closure"></a>
Rust's closures are anonymous functions that you can store in a variable or pass as arguments to other functions. Closures capture values from  the scope in which they're called, enabling code reuse and behavior customization.

```
let closure_example = |param| {
    println!("This is just an example closure");
    param
}
closure_example(15);
```

Closures are usually short and relevant only within a narrow context rather than in any arbitrary scenario. Within these limited contexts, the compiler is reliably able to infer the types of the parameters and the return type, similar to how it’s able to infer the types of most variables.

As with variables, we can add type annotations if we want to increase explicitness and clarity at the cost of being more verbose than is strictly necessary. 

```
let closure_example_annotated = |param: u32| -> u32 {
    println!("Annotated example");
    param
}
```

Closure definitions will have one concrete type inferred for each of their parameters and for their return value. The compiler will not allow calling the same closure twice with two different inferred types (ie ```closure_example("fire")``` and then ```closure_example(32)```).

---
**Storing Closures Using Generic Parameters and the ```Fn``` Traits**<br>

The code pattern known as *memoization* or *lazy evaluation* involves creating a struct to hold the closure as well as the value returned by calling the closure. Specifically, the struct executes the closure only if we need the return value and it will cache the result, thereby enabling the rest of the code to reuse this result.

Every struct definition requires the types for each of its fields. To define structs, enums, or function parameters that use closures, we use generics and trait bounds. All closures implement at least one of the traits: ```Fn```, ```FnMut``` or ```FnOnce```.

We add types to the ```Fn``` trait bound to represent the types of the parameters and return values the closures must have to match this trait bound. 

```
struct Cacher<T>
    where T: Fn(u32) -> u32
{
    calculation: T,
    value: Option<u32>,
}
```

This ```Cacher``` struct has a ```calculation``` field of the generic type ```T```. The trait bounds on ```T``` specify that it's a closure by using the ```Fn``` trait. 

> While functions can implement all of the ```Fn``` traits, closures are better for caputuring a value from the environment.

The ```value``` field has type ```Option<32>```. Before executing the closure, the ```value``` is ```None```. Once code using ```Cacher``` asks for the *result* of the closure, the ```Cacher``` executes the closure and stores the result in a ```Some``` variant in the ```value``` field. This ensures that if the code asks for the result of the closure again, ```Cacher``` can return the result stored in the ```Some``` variant. Here is the logic in code form:

```
impl<T> Cacher<T>
    where T: Fn(u32) -> u32
{
    fn new(calculation:T) -> Cacher<T> {
        Cacher {
            calculation,
            value: None,
        }
    }

    fn value(&mut self, arg: u32) -> u32 {
        match self.value {
            Some(v) => v,
            None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
            },
        }
    }
}
```

The ```Cacher::new``` function takes a generic parameter ```T```, which we've defined as maintaining the same trait bound as the ```Cacher``` struct. ```Cacher::new``` returns a ```Cacher``` instance which holds the closure that was specified in the ```calculation``` field as well as ```None``` in the ```value``` field (because we haven't executed the closure yet). 

> The fields for ```Cacher``` are kept private because we want to manage the struct fields' values instead of letting the calling code change these values directly.

Even so, this implementation suffers a fundamental problem whereby if we call the closure with one value, then the value is cached and we can no longer call the closure with a new value. We can modify this implementation to alleviate this problem by holding a hash map instead of a single value. We can also introduce more generic parameters to increase the flexibility of this implementation (thereby allowing it to accept and/or return more than one type). *I plan to explore these improvements in a workout app, which may be pushed to my github soon.*

---
**Tradeoffs for Using Closures vs Functions**<br>
When a closure captures a value from its environment, it uses memory to store the values for use in te closure body. This incurs more memory overhead than a normal function, which does not capture its environment.

Closures can capture values from their environment in three ways, each of which directly map to the ways that a function can take a parameter:
1. **taking ownership** => ```FnOnce``` consumes the variables it captures from its enclosing scope (otherwise known as the closure's *environment*). To consume the captured variables, the closure must take ownership of these variables and move them into the closure once it is defined. 
> The ```Once``` part of the name indicates that the closure can't take ownership of the same variables more than once (so it can be called only once).<br>
2. **borrowing mutably** => ```FnMut``` can change the environment because it mutably borrows values.
3. **borrowing immutably** => ```Fn``` borrows values from the environment immutably.

> When you create a closure, Rust will infer which trait to use based on how the closure manages the variables from the environment. All closures implement ```FnOnce```, but closures that don't move the captured variables also implement ```FnMut```, and closures that don't need mutable access to the captured variables also implement ```Fn```. 

We use the ```move``` keyword before the parameter list to force closures to take ownership of the values that it uses in the environment. Example:

```
fn main() {
    let x = vec![1, 2, 3];

    let equal_to_x = move |z| z == x;

    println!("can't use x here: {:?}", x); 
    // the above line will most definitely will give us a compilation error
}
```

> Easy way to start with closures is to use ```Fn``` and the compiler will tell you whether you need to add ```FnMut``` or ```FnOnce``` based on what happens inside the closure body. 

## Processing a Series of Items with Iterators <a name="iterator">

In Rust, iterators are *lazy* such that they have no effect until we call methods that consume the iterator (to use it). Here's the syntax for creating an iterator over the items in a vector.

```
let v1 = vec![1, 2, 3];

let v1_iter = v1.iter();

for val in v1_iter {
    println!("Got: {}", val);
}
```

---
**The ```Iterator``` Trait and the ```next``` Method**<br>
All iterators implement the trait ```Iterator```, which has the definition:

```
trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // methods with default implementations elided
}
```

This definition tells us that implementing the ```Iterator``` trait requires defining an ```Item``` type, which is used in the return type of the ```next``` method. 

> The ```Iterator``` trait only requires implementators to define one method: the ```next``` method, which returns one item of the iterator at a time, which is wrapped in ```Some``` and, when the iteration is over, returns ```None```.

Although we don't need to make an iterator mutable for a ```for``` loop (because the loop takes ownership and makes it mutable behind the scenes), we would need to define the iterator as mutable if we are successively calling ```next``` (because each ```next``` call eats up an item from the iterator).

> The values we get from calls to ```next``` are immutable references to the values in the vector. The ```iter``` method produces an iterator over immutable references. If we want to create an iterator that takes ownership of ```v1``` and returns owned values, we can call ```into_iter``` instead of ```iter```. If we want to iterate over mutable references, we can call ```iter_mut``` (instead of ```iter```).

---
**Methods that Consume the Iterator**<br>
The ```Iterator``` trait maintains default methods provided by the standard library, some of which call the ```next``` method in their definition. Methods that call ```next``` are referred to as *consuming adaptors* because calling them uses up the iterator. An example is the ```sum``` method, which takes ownership of the iterator and iterates through items by repeatedly called ```next``` (which therefore consumes the iterator upon each call). As it iterates through the given object (maybe a vector), it adds each item to a running total and returns the total once the iteration is complete.

```
#[test]
fn iterator_sum() {
    let v1 = vec![1, 2, 3];
    
    let v1_iter = v1.iter();

    let total: i32 = v1_iter.sum();

    assert_eq!(total, 6);
}
```

> Note that we cannot use ```v1_iter``` after the call to ```sum``` because ```sum``` takes ownership of the iterator that we call.

---
**Methods that Produce Other Iterators**<br>
Other methods defined on the ```Iterator``` trait known as *iterator adaptors* enable you to change iterators into different kinds of iterators. Because all iterators are lazy, you still need to call one of the consuming adaptor methods to get results from calls to iterator adaptors.

```
let v1: Vec<i32> = vec![1, 2, 3];

v1.iter().map(|x| x + 1);
```

The above code calls the iterator adaptor ```map```, which takes a closure to call on each item to produce a new iterator. but the closure we've specified never gets called because **iterator adaptors are lazy** (so we need to consume the iterator). We can fix this by using the ```collect``` method to consume the iterator and colllect the resulting variables into a collection data type.

```
let v1: Vec<i32> = vec![1, 2, 3];

let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

assert_eq!(v2, vec![2, 3, 4]);
```

Because ```map``` takes a closure, we can specify any operation that we want to perform on each item. This provides a solid example of how closures enable customized behavior while also reusing the iteration behavior provided by the ```Iterator``` trait.

---
**Using Closures that Capture Their Environment**<br>
The ```filter``` iterator adaptor is a method on an iterator. ```filter``` takes a closure that takes each item from the iterator and returns a Boolean. If the closure returns ```true```, the  value will be included in the iterator produced by ```filter```. Conversely, if the closure returns ```false```, the value cannot be included in the resulting iterator. Here is an example with syntax:

```
#[derive(PartialEq, Debug)]
struct Student {
    name: String,
    score: u32,
}

fn pass_or_fail(students: Vec<Student>, passing_score: u32) -> Vec<Student> {
    students.into_iter()
        .filter(|s| s.score >= passing_score)
        .collect()
}

#[test]
fn filter_by_score() {
    let students = vec![
        Student { name: String::from("Bob"), score: 2300 },
        Student { name: String::from("Alice"), score: 2250 },
        Student { name: String::from("Charlie"), score: 1500 },
    ];

    let passing = pass_or_fail(students, 2000)

    assert_eq!(
        passing,
        vec![
            Student { name: String::from("Bob"), score: 2300 },
            Student { name: String::from("Alice"), score: 2250 },
        ]
    );
}
```

---
**Creating Our Own Iterators with the ```Iterator``` Trait**<br>
If we want to create our own iterators using the ```Iterator``` trait, we only need to provide a definition for the ```next``` method, and we can use all other methods that have default implementations provided by the ```Iterator``` trait. Here's a super simple example:

```
struct Counter {
    count: u32,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        self.count += 1;

        if self.count < 6 {
            Some(self.count)
        } else {
            None
        }
    }
}
```

Here, we set the associated ```Item``` type for our iterator to ```u32```. This indicates that the iterator will return ```u32``` values. We want our iterator to add 1 to the current state so we initialized ```count``` to 0 so it returns 1 first. If the value of ```count``` is less than 6, ```next``` will return the current value wrapped in ```Some```, but if ```count``` is 6 or higher, our iterator will return ```None```.

Because we already implemented the ```Iterator``` trait by defining the ```next``` method, we now can use any of the ```Iterator``` trait method's default implementations. As an example, let's say that we want to take the values produced by an instance of ```Counter```, pair them with values produced by another ```Counter``` instance after skipping the first value, multiply each pair together, keep only those reulsts that are divisible by 3, and add all the resulting values together, we could do as like this:

```
#[test]
fn using_other_iterator_trait_methods() {
    let sum: u32 = Counter::new().zip(Counter::new().skip(1))
                                    .map(|(a, b)| a * b)
                                    .filter(|x| x % 3 == 0)
                                    .sum();
    assert_eq!(18, sum);
}
```
> Side Note: consider using [this](https://doc.rust-lang.org/book/2018-edition/ch13-03-improving-our-io-project.html) to improve my zencryption command line tool by incorporating closures and iterators

### Comparing Performance: Loops vs Iterators <a name="loopsvsiterators"></a>

Although iterators are a high-level abstraction, they are compiled down to roughly the same code as if you'd written the lower-level code yourself. Specifically, iterators are one of Rust's *zero-cost abstractions* (<=> the abstraction imposes no additional runtime overhead). Bjarne Stroustrup defined *zero-overhead* in "Foundations of C++"(2012) 
> "In general, C++ implementations obey the zero-overhead principle: What you don't use, you don't pay for. And further: What you do use, you couldn't hand code any better."

When Rust knows the number of iterations for a loop, it can "unroll" the loop. *Unrolling* is an optimization that removes the overhead of the loop controlling code and instead generates repititive code for each iteration of the loop.

## Cargo and Crates <a name="cargo"></a>

> For more details, check out [Cargo Documentation](https://doc.rust-lang.org/cargo/)

*Release profiles* in Rust are predefined and customizable profiles with different configurations that allow a programmer to have more control over various options for compiling code. Each profile is configured independently of the others!

Cargo has two main profiles:
1. the ```dev``` profile is used implicitly whenever you run ```cargo build```
2. the ```release``` profile requires invoking the ```--release``` flag (```cargo build --release```)

Here are the default values for the ```opt-level``` setting for the ```dev``` and ```release``` profiles in the Cargo.toml file. The ```opt-level``` setting controls the number of optimizations that Rust applies to the code (ranging from 0 to 3). Although applying more optimizations enables faster runtime, it also extends compiling time (so if you're in development mode, it's better to pick a lower ```opt-level```).

```
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

---
**Making Useful Documentation Comments**<br>
Documentation comments use three slashes ```///``` instead of two and support Markdown notation for formatting the text. Place documentation comments right before the item that they're documenting. Here's an example (some markdown inception LOL):

```
/// Adds one to the number given
///
/// # Examples
///
/// ```
/// let five = 5;
///
/// assert_eq!(6, my_crate::add_one(5));
/// ```
pub fn add_one(x: i32) -> i32 {
    x + 1
}
```

We can generate the HTML documentation from this documentation comment by running ```cargo doc```. This command runs the ```rustdoc``` tool distributed in Rust and puts the generated HTML documentation in the *target/doc* directory. Running ```cargo doc --open``` builds the HTML and opens the result in a web browser.

Here are a few sections that crate authors commonly use in their documentation:
* **Examples**
* **Panics**: the scenarios in which the function being documented could panic
* **Errors**: if the function returns a ```Result```, describing the kinds of errors that may occur and what conditions might cause those errors to be returned can be helpful to callers so that they can write code to handle the different potential errors.
* **Safety**: If the function is ```unsafe``` to call, there should be a section explaining why the function is unsafe as well as which invariants the function expects the callers to uphold.

> Interestingly, runnning ```cargo test``` will run the code examples in the documentation as tests. This ensures that the examples work with the code.

Comments that appear after ```//!``` provide documentation for the item that contains the comments itself. Typically, these style comments are included at the top of the crate root file (*src/lib.rs*). Documentation comments within items are useful for describing crates and modules especially. Use them to explain the overall purpose of the container to help your users understand the crate's organization.

---
**Exporting a Convenient Public API with ```pub use```**<br>
You can re-export items to make a public structure that's different from the private structure by using ```pub use```. Re-exporting takes a public item in one location and makes it public in another location (as if it were defined in the other location instead).

Utilizing ```pub use``` enables flexibility with respect to structuring the crate internally by allowing you to decouple the internal strcuture from the API presented to users.

To learn how to publish crates to crates.io look [here](https://doc.rust-lang.org/book/2018-edition/ch14-02-publishing-to-crates-io.html).

### Cargo Workspaces <a name="workspace"></a>

A *workspace* is a set of packages that share the same *Cargo.lock* and output directory. An example structure of a workspace could consist of a binary and two libraries such that the binary depends on both libraries.

This section may be more useful in the future and, at that point in time, I can review [this section](https://doc.rust-lang.org/book/2018-edition/ch14-03-cargo-workspaces.html) in [The Book](https://doc.rust-lang.org/book/2018-edition/).

---
**Installing Binaries from Crate.io with ```cargo install```**<br>
All binaries installed with ```cargo install``` are stored in the installation root’s *bin* folder. If you installed Rust using *rustup.rs* and don’t have any custom configurations, this directory will be *$HOME/.cargo/bin*. Ensure that directory is in your ```$PATH``` to be able to run programs you’ve installed with ```cargo install```.

## Smart Pointers <a name="smartpointers"></a>
*Smart Pointers* are data structures that act as pointers while also maintaining additional metadata and capabilities. While references are pointers that only borrow data, smart pointers *own* the data they point to. Examples of smart pointers already covered include ```String``` and ```Vec<T>``` (because they own some memory and allow you to manipulate it). In addition, both of these types have metadata (ie their capacity) as well as extra capabilities and guarantees (ie ```String``` ensures that it will always be valid UTF-8).

Generally, smart pointers are implemented using structs that implement the ```Deref``` and ```Drop``` traits. The ```Deref``` trait enables an instance of the smart pointer to behave as a reference, which allows code that works with either references or smart pointers. On the other hand, ```Drop``` trait allows you to customize the code that is run when an instance of the smart pointer goes out of scope.

Most common smart pointers in the standard library: <br>
* [```Box<T>```](#boxt) for allocating values on the heap 
* [```Rc<T>```](#rct), which is a reference counting type that enables multiple ownership
* [```Ref<T>```](#reft) and ```RefMut<T>``` (accessed through ```RefCell<T>```), a type that enforces the borrowing rules at runtime instead of compile time.

> *interior mutability pattern* such that an immutable type exposes an API for mutating an interior value

> *reference cycles*: how they can leak memory and how to prevent them

### Using ```Box<T>``` to Point to Data on the Heap <a name="boxt"></a>
Boxes (denoted as type ```Box<T>```) allow you to store data on the heap rather than the stack. Even so, the pointer to the heap data is stored on the stack. For this reason, boxes don't have performance overhead (other than storing their data on the heap instead of on the stack). Examples of common use cases:
* When you have a type whose size can't be known at compile time and you want to use a value of that type in a context that requires an exact size
* When you have a large amount of data and you want to transfer ownership but ensure the data won't be copied
* When you want to own a value and you care only that it is a type that implements a specific trait instead of being a particular type

```
fn main() {
    let b = Box::new(5);
    println!("b = {}", b);
}
```
Here we store an ```i32``` value on the heap using a box. The program prints ```b = 5``` (so we can access the data in the box similar to how we would if this data were on the stack). Similar to owned values, when a box goes out of scope, it is deallocated (both for the box stored on the stack and the data it points to on the heap).

---
**Enabling Recursive Types with Boxes**<br>
At compile time, Rust must know how much space a type takes up, but this doesn't work for *recursive types* (ie where a value can have as part of itself another value of the same type). This nesting of values makes it impossible for Rust to effectively allocate space. Even so, boxes have a known size so we can insert a box in a recursive type definition to enable recursive types.

**Cons List**<br>
A *cons list* is a data structure from Lisp. The cons function is used the following context: "to cons *x* onto *y*", which means to construct a new container instance by putting the element *x* at the start of this new container, followed by the container *y*.  Each item in a cons list contains two elements: the value of the current item and the next item. The last item in the list contains only a ```Nil``` value without any next item. 

To demonstrate what Boxes can do, we'll implement a cons list that holds only ```i32``` values. 

```
enum List {
    Cons(i32, List),
    Nil
}

use List::{Const, Nil};

fn main() {
    let list = Cons(1, Cons(2, Cons(3, Nil)));
}
```
**This won't compile** because there's no indication of how much space is required to store a ```List``` variant that is recursive (because it holds another value of itself directly). Instead, we can use ```Box<T>``` to get a recursive type with a known size. 

Rust knows how much space is necessary to store a ```Box<T>``` because this is just a pointer to data stored in the heap (and a pointer's size doesn't change based on the data that it's pointing to). To fix our code, we can put a ```Box<T>``` inside our ```Cons``` variant (instead of another ```List``` value directly). The ```Box<T>``` pints to the next ```List``` value that will be on the heap rather than inside the ```Cons``` variant. 

```
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};

fn main() {
    let list = Cons(1,
        Box::new(Cons(2,
            Box::new(Cons(3, 
                Box::new(Nil))))));
}
```

This ```Cons``` variant requires the size of an ```i32``` as well as the space required to store the box's pointer data. Because the ```Nil``` variant doesn't store any values, it needs less space than the ```Cons``` variant. This tells us that any ```List``` value will take up the size of an ```i32``` as well as the size of a box's pointer data.

> Boxes only provide the indirection and box allocation (so they don't have any other special capabilities like those of other smart pointer types). Likewise, they don't have the performance overhead incurred by the special capabilities of other smart pointer types. 

When a ```Box<T>``` value goes out of scope, the heap data that the box is pointing to is cleaned up because of the ```Drop``` trait implementation. ```Box<T>``` is treated like a reference because it implements the ```Deref``` trait.

---
**Treating Smart Pointers like Regular References with the ```Deref``` Trait**<br>
We can customize the behavior of the *dereference operator*, ```*```, by implementing the ```Deref``` trait. Specifically, implementing this trait makes it possible for smart pointers to work in a similar way as references. 

```
fn main() {
    let x = 5;
    let y = &x;
    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```
This is the syntax of using a dereference operator to follow a reference to an ```i32``` value. Without the dereference operator, we would get a compiler error that tells us that we can't compare a reference to an integer.

We can rewrite this code with ```Box<T>``` instead of reference. 

```
fn main() {
    let x = 5;
    let y = Box::new(x);
    
    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```
We can use the dereference operator to follow the box's pointer in the same way that we did when ```y``` was a reference. The only difference with this code is that ```y``` is set to be an instance of a box pointing to the value in ```x``` (instead of a reference pointing to the value of ```x```).

---
**Defining Our Own Smart Pointer**<br>

```
struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &T {
        &self.0
    }
}
```
The ```type Target = T;``` syntax defines an associated type for the ```Deref``` trait to use. Associated types are a slightly different way of declaring a generic parameter. The body of the ```deref``` method contains ```&self.0```, which ensures that ```deref``` returns a reference to the value we want to access with the ```*``` operator.

> The ```deref``` method provides the compiler with the ability to take a value of any type that implements ```Deref``` and call the ```deref``` method to get a ```&``` reference that it knows how to dereference.

> When we entered ```*y``` in the above code, Rust actually runs this code behind the scenes: ```*(y.deref())```. Specifically, Rust substitutes the ```*``` operator with a call to the ```deref``` method, and then a plain dereference so that we don't have to consider whether or not we need to call the ```deref``` method. 

---
**Implicit Deref Coercions with Functions and Methods**<br>
*Deref coercion* converts a reference to a type that implements ```Deref``` into a reference to a type that ```Deref``` can convert the original type into. Deref coercion occurs automatically when we pass a reference to a particular type's value as an argument to a function or method that doesn't match the parameter type in the function or method definition. A sequence of calls to the ```deref``` method converts the type we provided into the type required by the parameter.

To see how deref coercion works, we can utilize the ```MyBox<T>``` type that we previously defined. 

```
// -- snip definition of MyBox<T> --

fn hello(name: &str) {
    println!("Hello, {}!", name);
}

fn main() {
    let m = MyBox::new(String::from("Rust"));
    hello(&m);
}
```

Calling ```hello``` with a reference to a ```MyBox<String>``` value works because of deref coercion. Indeed, we are calling the ```hello``` function with ```&m```, a reference to a ```MyBox<String>```. Because we implemented ```Deref``` on ```MyBox<T>```, Rust can convert ```&MyBox<String>``` to ```&String``` by calling ```deref```.  Rust calls ```deref``` again to turn the ```&String``` to ```&str``` (which matches the ```hello``` function's definition).

Here's how the code would work if Rust didn't implement deref coercion...ew!
```
fn main() {
    let m = MyBox::new(String::from("Rust"));
    hello(&(*m)[..]);
}
```
For clarity, ```(*m)``` dereferences the ```MyBox<String>``` into a ```String```. Then the ```&``` and ```[..]``` take a string slice of the ```String``` that is equalt o the whole string to match the signature of ```hello```. 

> When the ```Deref``` trait is defined for the types involved, Rust will analyze the types and use ```Deref::deref``` as many times as necessary to get a reference to match the parameter's type. 
> Because the number of times that ```Deref::deref``` needs to be inserted is resolved at compile time, there is no runtime penalty for taking advantage of deref coercion.

Rust does deref coercion when it finds types and trait implementations in three cases:
* From ```&T``` to ```&U``` when ```T: Deref<Target=U>```
* From ```&mut T``` to ```&mut U``` when ```T: DerefMut<Target=U>```
* From ```&mut T``` to ```&U``` when ```T:Deref<Target=U>```

While Rust will coerce a mutable reference to an immutable one, the reverse is not allowed: immutable references will never coerce to mutable references. This follows from the borrowing rules that mandates that, if you have a mutable reference, it must be the only reference to the data. The Rust compiler can't guarantee that the immutable reference (that is being converted to an immutable reference) is the only immutable reference (which would be necessary because we are instantiating an immutable reference).

---
**Running Code on Cleanup with the ```Drop``` Trait**<br>
Implementing the ```Drop``` trait allows you to customize what happens when a value is about to go out of scope. This trait is almost always used in the context of implementing smart pointers, but it can also be used to release resources like files or network connections. ```Box<T>``` customizes ```Drop``` to deallocate the space on the heap to which the box points.

> In other languages, the programmmer has to call code to free memory or resources every time they finish using an instance of a smart pointer. Conversely, in Rust, you can specify that some code must be run whenever a value goes out of scope and the compiler will insert this code automatically. This ensures against the leaking of resources.

To use the ```Drop``` trait, you must implement one method named ```drop``` that takes a mutable reference to ```self```.  As an example, we'll implement a ```CustomSmartPointer``` struct whose only custom functionality is that it will print ```Dropping CustomSmartPointer!``` when the instance goes out of scope.

```
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created.");
}
```

Rust automatically called ```drop``` for us when our instances went out of scope (calling the code specified in our implementation of the ```Drop``` trait). Variables are dropped in the reverse order of their creation (ie ```d``` is dropped before ```c```). 

---
**Dropping a Value Early with ```std::mem::drop```**<br>
Sometimes we may want to force the ```drop``` method. An example may be if the smart pointer manages locks and we want to release the lock so other code in the same scope can acquire the lock. In this situation, Rust doesn't allow us to call the ```Drop``` trait's ```drop``` method manually; instead we have to call the ```std::memory::drop``` function provided by the standard libray if we want to force a value to be dropped before the end of its scope.

Rust doesn't let us call ```drop``` explicitly because Rust would still automatically call ```drop``` on the value at the end of ```main```. This would cause a *double free error* (because Rust would be trying to clean up the same value twice). Here's an example of how we could apply ```std::mem::drop``` to explicitly drop a value before it goes out of scope. 

```
fn main() {
    let c = CustomSmartPointer { data: String::from("some data") };
    drop(c);
}
```


### ```Rc<T>``` <a name="rct"></a>

### ```Ref<T>``` <a name="reft"></a>


