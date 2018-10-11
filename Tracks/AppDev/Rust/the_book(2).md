# Coding Rust Cheatsheet (2 of 2)

Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it.

These are notes from [The Rust Book](https://doc.rust-lang.org/book/), but they also may draw from [Steve Donovan's Gentle Intro](https://stevedonovan.github.io/rust-gentle-intro/readme.html). **These notes only cover the last 7 chapters of The Book** 

* [Closure](#closure)

## Closures <a name="closure"></a>
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