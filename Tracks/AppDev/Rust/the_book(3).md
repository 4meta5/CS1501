# Coding Rust Cheatsheet (3 of 3)

Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it.

These are notes from [The Rust Book](https://doc.rust-lang.org/book/), but they also may draw from [Steve Donovan's Gentle Intro](https://stevedonovan.github.io/rust-gentle-intro/readme.html). **These notes only cover the last 7 chapters of The Book** 

* [OOP](#oop)

## Object Oriented Programming Features of Rust <a name="oop"></a>

The *Gang of Four* book defines OOP as:
> Object-oriented programs are made up of objects. An *object* packages both data and the procedures that operate on that data. The procedures are typically called *methods* or *operations*.

Using this definition, Rust is object oriented: structs and enums have data, and ```impl``` blocks provide methods on structs and enums.


# I DECIDED TO PAUSE BOOK NOTES AND START BUILDING A FEW PWASM TUTORIALS