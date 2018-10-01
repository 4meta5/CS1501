# Rust Code Patterns
* [Writing to a File](#fileWrite)
* [Enums and Structs](#es)
* [Unique Type Validation](#validation)


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
