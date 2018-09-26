# Coding Rust Cheatsheet (WIP)

Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it.

These are notes from [The Rust Book](https://doc.rust-lang.org/book/). They start from the middle of the Chapter 2 ```guessing_game``` tutorial.
* [Guessing Game](#guess)
* [Immutability](#immutability)
    * [Variables and Type Annotations](#variables)
    * [Shadowing](#shadowing)
    * [Ownership](#ownership)
* [Basics](#basics)
    * [The Stack and the Heap](#stackheap)
    * [Using Modules to Reuse and Organize Code](#modules)
* [Data Structures](#datastructures)
    * [Structs](#structs)
    * [Enums and Pattern Matching](#enums)
    * [Common Collections](#collections)
* [Error Handling](#error)
    * [Generics](#generic)

## Guessing Game (Ch.2) <a name="guess"></a>
Switching from an ```expect``` call to a ```match``` expression is how you generally move from crashing on an error to handling the error.


## Immutability <a name="immutablity"></a>
* [Variables and Type Annotations](#variables)
* [Shadowing](#shadowing)
* [Ownership](#ownership)

### Variables and Type Annotations <a name="variables"></a>
* Variables are by default immutable.
* Rust has two compound types: tuples and arrays. In Rust, both are fixed length.
* Type annotations enable static checking. 

Consider comparing performance for different data structures between mutable implementations and *functional*-style code that relies on immutability (which is safer for concurrency).

### Shadowing <a name="shadowing"></a>
Shadowing is different than marking a variable as ```mut```, because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the ```let``` keyword. By using ```let```, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed. [Chapter 3 of The Book](https://doc.rust-lang.org/book/2018-edition/ch03-01-variables-and-mutability.html)

### Ownership <a name="ownership"></a>
Some languages (ie Java) have garbage collection that constantly looks for memory that is no longer being used. In other languages (C++), the programmer must explicity allocate and deallocate (free) memory. In **Rust**, memory is managed through a system of ownership with a set of rules that the compiler checks at compile time.

Rules: <br>
1. Each value in Rust has a variable that's called its *owner*.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

For **string literals**, the textis hardcoded directly into the final executable, enabling us to store this data on the stack. Although its immutability limits what we can do with this data type, storing string literals on the stack means that string literals are fast and efficient. Conversely, **String type** supports a mutable, growable piece of text by allocating memory on the heap, unknown at compile time, to hold the contents. This means that the memory must be requested from the OS at runtime. 

In Rust, garbage collection is managed by deallocating memory in the heap whenever a variable goes out of scope. When a variable goes out of scope, Rust calls ```drop``` to deallocate its memory. This pattern of deallocating resources at the end of an item's lifetime is known as *Resource Acquisition is Initializaion (RAII)*.

If you've heard the terms *shallow copy* and *deep copy* while working with other languages, the concept of copying the pointer, length, and capacity without copying the data probably sounds like making a shallow copy. However, Rust only enables moves and not shallow copies because the pointer is invalidated as soon as the data is removed. This prevents *double free error*.

Rust has a special annotation called the ```Copy``` trait that we place on types like integers that are stored on the satck. If a type has the ```Copy``` trait, an older variable is still usable after assignment. Rust won't let us annotate a type with the ```Copy``` trait if the type, or any of its parts, has implemented the ```Drop``` trait. Here are some of the types that are ```Copy```: <br>
* All the integer types, such as ```u32```
* The Boolean type, ```bool```, with values ```true``` and ```false```.
* All the floating point types, such as ```f64```.
* The character type, ```char```.
* Tuples but only if they contain types that are also ```Copy```. For example, ```(i32, i32)``` is ```Copy```, but not ```(i32, String)```.

The ownership of a variable follows the same pattern every time: assigning a value to another variable moves it. When a variable that includes data on the heap goes out of scope, the value will be cleaned up by ```drop``` unless the data has been moved to be owned by another variable.

---

**References and Borrowing** <br>

We use **references** to pass a value to a function without transferring ownership.

```
fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // Here, s goes out of scope. But because it does not have ownership of what
  // it refers to, nothing happens.
```
Using references as function parameters is known as **borrowing**. References are immutable by default, but mutable references are possible. The only limitation is that you can only have one mutable reference per piece of data in a scope. 
> We can use curly brackets to create a new scope, allowing for multiple mutable references, just not *simultaneous* ones.

This allows for mutation, but in a very controlled fashion. This allows Rust to prevent **data races** at compile time. A data race is a race condition in which: <br>
* Two or more pointers access the same data at the same time.
* At least one of the pointers is being used to write to the data.
* There's no mechanism being used to synchronize access to the data.

---

**Slices**<br>
```
let hello = &s[0..5];
let world = &s[6..11];
```
```
let hello = &s[0..=4];
let world = &s[6..=10];
```
They're the same; the top is noninclusive for the last number and the second syntax is inclusive for the second number.

String literals are immutable because ```&str``` (string literal type) is an immutable reference.

## Basic Programming Concepts <a name="basics"></a>
* [The Stack and the Heap](#stackheap)
* [Using Modules to Reuse and Organize Code](#modules)

### The Stack and the Heap <a name="stackheap"></a>
In a systems programming language like Rust, whether a value is on the stack or the heap influences how you can interact with it.

The **stack** stores values in the order it gets them and removes the values in the opposite order (LIFO = last in, first out). The stack is fast because of the way it accesses the data: it never has to search for a place to put new ata or a place to get data from because it can only pop or push values from the top of the stack. All data on the stack must take up a known, fixed size.

When the size of data is unknown at compile time, this data is stored on the **heap**. To put data on the heap, you request some space from the OS and the OS provides a pointer to the address of that location. This process is called *allocating on the heap*. Accessing data in the heap is slower than accessing data on the stack because you have to follow a point to get there. Allocating a large amount of space on the heap can also take time

### Using Modules to Reuse and Organize Code <a name="modules"></a>
A **module** is a namespace that contains definitions of functions or types, and you can choose whether those defintions are visible outside their module using the ```pub``` keyword. 
* The ```mod``` keyword declares a new module.
* By default, functions, types, constants, and modules are private. 
* The ```use``` keyword brings modules or the definitions inside modules, into scope so it's easier to refer to them.

The rules of modules with respect to files:
* If a module named ```foo``` has no submodules, you should put the declarations for ```foo``` in a file named *foo.rs*.
* If a module named ```foo``` does have submodules, you should put the declarations for ```foo``` in a file named *foo/mod.rs*.

**Privacy Rules**: <br>
* If an item is **public**, it can be accessed through any of its parent modules.
* If an item is **private**, it can be accessed only by its immediate parent module and any of the parent's child modules.

**Bringing Names into Scope with the ```use``` Keyword** <br>
```
pub mod laugh {
    pub mod at {
        pub mod me {
            pub fn now() {}
        }
    }
}

use laugh::at::me;

fn main() {
    me::now();
}
```
The ```use``` keyword brings only what we've specified into scope: it does not bring children of modules into scope.

Because enums also form a similar namespace to modules, we can bring an enum's variants into scope with ```use``` as well. For any kind of ```use``` statement, if you're bringing multiple items from a namespace into scope, you can use this syntax:
```
enum Fruits {
    Apple,
    Orange,
    Grape,
}

use Fruits::{Apple, Orange};

fn main() {
    let apple = Apple;
    let orange = Orange;
    let grape = Fruits::Grape;
}
```

We can bring all items in a namespace into scope at once using the ```*``` (glob) syntax.
```
use Fruits::*;
```

To access a parent module, you use ```super``` to move up one module in the hierarchy from the current module
```
super::individual::work();
```

We can also use leading colons to let Rust know that we want to start from the root and list the whole path:
```
::individual::work();
```

## Data Structures <a name="datastructures"></a>
* [Structs](#structs)
* [Enums and Pattern Matching](#enums)
* [Common Collections](#collections)

### Structs <a name="structs"></a>
This shows how the struct update syntax looks.
```
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};
```

**Methods** are similar to functions: except they're defined within the context of a struct and their first parameter is always ```self``` (which represents the instance of the struct the method is being called on).
```
struct Block {
    hash_pointer: u32,
    merlkle_root_hash: u32,
}

impl Block {
    fn printMerkle(&self) -> u32 {
        println!("Merkle root hash is {}", self.merkle_root_hash);
        self.merkle_root_hash
    }
}
```

The main benefit of using methods instead of functions, in addition to using method syntax and not having to repeat the type of ```self``` in every method’s signature, is for organization. We’ve put all the things we can do with an instance of a type in one ```impl``` block rather than making future users of our code search for capabilities of ```Rectangle``` in various places in the library we provide.

Structs let you create custom types that are meaningful for your domain. By using structs, you can keep associated pieces of data connected to each other and name each piece to make your code clear. Methods let you specify the behavior that instances of your structs have, and associated functions let you namespace functionality that is particular to your struct without having an instance available.

## Enums and Pattern Matching <a name="enums"></a>
```
enum Mood {
    Happy,
    Sad,
    Mad,
}
```
Mood is now an enumeration type with variants ```{Happy, Sad, Mad}```. The variants of the enum are namespaced under the identifier (```Mood```). This allows us to define special functions that take any variant of the same enum by specifying the enum type for the function's input.

The way we create instants of the variats is with double colons:
```
let amars_happy = Mood::Happy;
let amar_mad = Mood::Mad;
```

Advantage for **Enums over Structs**: Enums can have variants of different types and amounts of associated data.

A common code pattern is to define structs and use these structs to store data in the variants of an enum:
```
struct ProofOfStake {
    cap_req:        u32,
    inflation_rate: u32,
    tps:            u32,
    -- more code -- 
}

struct ProofOfWork {
    hash_rate:      u32,
    avg_fee:        u32,
    tps:            u32,
    -- more code --
}

enum BlockchainConsensus {
    Ethereum(ProofOfStake),
    Bitcoin(ProofOfWork)
}
```

**Option Enum in Rust**<br>
In his 2009 presentation “Null References: The Billion Dollar Mistake,” Tony Hoare, the inventor of null, has this to say:
> I call it my billion-dollar mistake. At that time, I was designing the first comprehensive type system for references in an object-oriented language. My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn’t resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.

```
enum Option<T> {
    Some(T),
    None,
}
```

Because the ```Option<T>``` enum is so useful, it is does not need to be brought into scope and you can use its variants directly instead of referencing the identifier.
```
let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
```
When we have a ```Some``` value, we know that a value is present and the value is held within the ```Some```. When we have a ```None``` value, in some sense, it means the same thing as null: we don’t have a valid value. So why is having ```Option<T>``` any better than having null?

In short, because ```Option<T>``` and ```T``` (where ```T``` can be any type) are different types, the compiler won’t let us use an ```Option<T>``` value as if it were definitely a valid value. For example, this code won’t compile because it’s trying to add an ```i8``` to an ```Option<i8>```:
```
let x: i8 = 5;
let y: Option<i8> = Some(5);

let sum = x + y;
```

**How is this better than having a null value?**<br>
In order to have a value that can possibly be null, you must explicitly opt in by making the type of that value ```Option<T>```. Then, when you use that value, you are required to explicitly handle the case when the value is ```null```. Everywhere that a value has a type that isn’t an ```Option<T>```, you can safely assume that the value isn’t ```null```. This was a deliberate design decision for Rust to limit ```null```’s pervasiveness and increase the safety of Rust code.

---
**Match Expressions**
```
enum StudentYear {
    Freshman,
    Sophomore,
    Junior,
    Senior(Job),
}

fn class_number(studentYear: StudentYear) -> i8 {
    match studentYear {
        StudentYear::Freshman => 1,
        StudentYear::Sophomore => 2,
        StudentYear::Junior => 3,
        StudentYear::Senior => 4,
    }
}
```
Each of the match arms has two parts <br>
1. Pattern - the enum variant in this case
2. Arm - code to run if the pattern is matched

**Concise Control Flow with ```if let```**<br>
The ```if let``` syntax lets you combine ```if``` and ```let``` into a less verbose way to handle values that match one pattern while ignoring the rest. 
```
if let StudentYear::Senior = studentYear {
    println!("Student is in their {}th year", 4);
} else {
    println!("Student is NOT in their {}th year", 4);
}
```

## Common Collections <a name="collections"></a>

Collections can contain multiple values and the data that collections point to are **stored in the heap**.

[Collections](https://doc.rust-lang.org/std/collections/index.html) can be grouped into four major categories: <br>
* Sequences:  Vec, VecDeque, LinkedList <br>
* Maps:       HashMap, BTreeMap <br>
* Sets:       HashSet, BTreeSet <br>
* Misc:       BinaryHeap <br>


> "To get this out of the way: you should probably just use Vec or HashMap. These two collections cover most use cases for generic data storage and processing. They are exceptionally good at doing what they do. All the other collections in the standard library have specific use cases where they are the optimal choice, but these cases are borderline niche in comparison. Even when Vec and  HashMap are technically suboptimal, they're probably a good enough choice to get started." ~ [std::collections documentation](https://doc.rust-lang.org/std/collections/index.html)

Likewise, we'll discuss the three most common collections seen in Rust programs:<br>
* A **vector** allows you to store a variable number of values next to each other.
* A **String** is a collection of characters.
* A **hash map** allows you to associate a value with a particular key. It's a particular implementation of the more general data structure called a *map*.

---
```Vec<T>``` (vector) can only store values of the same type.  To create a new, empty vector, we call the ```Vec::new``` function.
```
let v: Vec<i32> = Vec::new(); //create a new, empty vector for holding values of type i32
```

We can also use the ```vec!``` macro to create a ```Vec<T>``` that has initial values. We can access the items in a vector using either the ```get``` method or the indexing syntax: <br>
```
let v = vec![1, 2, 3, 4, 5];
let s = &v[4];             //indexing syntax
let v_index = 2;

match v.get(v_index) {
    Some(_) => { println!("Reachable element at index: {}", v_index); },
    None => { println!("Unreachable element at index: {}", v_index); }
}
```

We can use an [enum](#enums) to store multiple types in a vector.

---
Strings are implemented as a collection of bytes. Rust only has one string type in the core language, the string slice ```str``` that is usually seen in its borrowed form ```&str```. Conversely, the ```String``` type is provided by Rust's standard library, and is a growable, mutable, owned UTF-8 encoded string type. To create a new, empty ```String```,
```
let mut s = String::new();
```
We can use the ```to_string``` method to create a ```String``` from a string literal.
```
let data = "initial contents";

let s = data.to_string();

// the method also works on a literal directly:
let s = "initial contents".to_string();
```
We can also use ```String::from``` to create a ```String``` from a string literal.
```
let s = String::from("initial contents");
```
We can grow a ```String``` by using the ```push_str``` mtethod to append a string slice.
```
let mut s = String::from("foo");
s.push_str("bar");
```
We can also use the ```+``` operator to add two strings, but this follows certain rules
```
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // Note s1 has been moved here and can no longer be used
```
Specifically...
```
fn add(self, s: &str) -> String {
```
Note that the compiler uses *deref coercion* to coerce ```&String``` into a ```&str```.

**Rust strings don't support indexing**. But why not? Let's consider **internal representation**. A ```String``` is a wrapper over a ```Vec<u8>```. UTF-8 complicates indexing because bytes don't always correspond to single letters. Therefore, Rust provides different ways of interpreting the raw string data that computers store so that each program can chose the interpretation it needs. 

The final reason that Rust doesn't allow us to index into a ```String``` to get a character is that indexing operations are expected to always take constant time ```(O(1))```. But it isn't possible to guarantee that performance with a ```String```, because Rust would have to iterate through all the contents to determine how many valid characters were therein.

---
The type ```HashMap<K, V>``` stores a mapping of keys of type ```K``` to values of type ```V```.

```
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10)
```
 This isn't as common as ```String``` or ```Vec<T>``` so it must be brought into scope with the ```use``` keyword.

 Like vectors, hash maps are homogeneous; all of the keys must have the same type, and all of the value must also have the same type.

 Another way of constructing hash maps is by using the ```collect``` method on a vector of tuples, where each typle consists of a key and a value. The ```collect``` method gathers data intoa  number of collection types, including ```HashMap```. 
 ```
use std::collections::HashMap;

let colors  = vec![String::from("Blue"), String::from("Yellow")];
let numbers = vec![8, 1];

let colorToNumbers: HashMap<_, _> = colors.iter().zip(numbers.iter()).collect();
 ```
The type annotation ```HashMap<_, _>``` is needed here because it's possible to ```collect``` into many different data structures; the underscores enable Rust to infer the types based on the data in the vectors.

**Hash Maps and Ownership**: For types that implement the ```Copy``` trait, like ```i32```, the values are copied into the hash map. For owned values like ```String```, the values will be moved and the hash map will be the owner of those values.

We can get a value out of the hash map by providing its key to the ```get``` method:
```
use std::collections::HashMap;

let mut ppg = HashMap::new();

ppg.insert(String::from("LBJ"), 30);
ppg.insert(String::from("Jordan"), 40);
ppg.insert(String::from("Amar"), 100);

let Jordan = String::from("Jordan");
let JordanPoints = ppg.get(&Jordan);
-- we also need to handle the Option<&V> returned with match presumably --
```
We can iterate over each key/value pair in a hash map using a ```for``` loop:
```
use std::collections::HashMap;

let mut totalETH = HashMap::new();

totalEth.insert(String::from("Amar"), 100);
totalEth.insert(String::from("Omar"), 1000000);

for (key, value) in &totalEth {
    println!("{}: {}", key, value);
}
```

To update a Hash Map, we can <br>
* replace the old value with the new value <br>
* keep the old value and ignore the new value (only adding if the key doesn't already have a value) <br>
* combine the old value and the new value <br>

## Error Handling <a name="error"></a>

Rust doesn't have *exceptions* like other languages. Instead, Rust has the type ```Result<T, E>``` for recoverable errors and the ```panic!``` macro that stops execution when the program encounters an unrecoverable error. 

---
When the ```panic!``` macro executes, your program will print a failure message, unwind, clean up the stack, and then quit. **Unwinding** in this context means that Rust walks back up the stack and cleans up the data from each function it encounters. The alternative to this is to immediately *abort*, which ends the program without cleaning up. 

We can set the ```RUST_BAKCTRACE``` environment variable to get a backtrace of exactly what happened to cause an error. A *backtrace* is a list of all the functions that have been called to get to this point. Backtraces in Rust work as they do in other languages: start from the bottom and read until you see files you wrote to find the error.

---
**Recoverable Errors with ```Result```**<br>
In many cases, it is better to respond to a function failure instead of requiring the program to stop entirely. An example of when this may be useful would be when you're automating the editing of files and need to ensure that if a file does not exist, then a new file can be created and this new file will be edited accordingly.

The ```Result``` enum is defined as having two variants, ```Ok``` and ```Err``` <br>
```
enum Result<T, E> {
    Ok(T)
    Err(E)
}
```
The ```T``` and ```E``` are [generic type parameters](#generic). ```T``` represents the value that will be returned in the ```Ok``` variant in the successful case and, in the event of failure, ```E``` is returned in the ```Err``` variant. Let's consider how this would work in the context of validating transactions (like a light client).
```
use blockchain::spv::Verify;

fn main() {
    let t = Verify::transaction(tx_data);

    let t = match t {
        Ok(tx_data) => tx_data,
        Err(error) => {
            panic!("There was a problem verifying the transaction data: {:?}", error)
        },
    };
}
```
> Like the ```Option``` enum, the ```Result``` enum and its variants have been imported in the prelude.

### Generic Types, Traits, and Lifetimes <a name="generic"></a>
