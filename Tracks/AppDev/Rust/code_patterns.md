# Rust Code Patterns
* [Writing to a File](#fileWrite)
* [Enums and Structs](#es)
* [Unique Type Validation](#validation)
* [Recursive Data Types](#recursive)

> need to create a tool for tracking code patterns; maybe create an mdbook that is relatively complicated

## Writing to a File <a name="fileWrite"></a>
```
use std::io;
use std::io::Read;
use std::fs;

fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```
Rust provides a convenient function called ```fs::read_to_string``` that will open the file, create a new ```String```, read the contents of the file, and put the contents into that ```String``` before returning it.

## Enums and Structs <a name="es"></a>

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

## Unique Type Validation <a name="validation"></a>

> Use ```new``` method on a struct (or whatever the type is)


## Recursive Data Types <a name="recursive"></a>

```Box<T>``` provides the simplest form of heap allocation in Rust. When the given object goes out of scope, ```Drop``` is called to deallocated the memory. We use this on recursive data types because it helps manage their lifetimes.

For example, a recursive list type is difficult in Rust because we must know how much space to allocate for the list (and therefore we must know the size). By using Rust, we can dynamically allocate memory in a relatively safe way.

```
#[derive(Debug)]
enum List<T> {
    Cons(T, Box<List<T>>),
    Nil,
}

fn main() {
    let list: List<i32> = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))));
    println!("{:?}", list);
}
```
> ```Box``` is a pointer type for heap allocation