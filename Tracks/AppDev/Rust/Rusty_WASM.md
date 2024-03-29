# Rust and WASM

Notes from [Rust and WebAssembly](https://rustwasm.github.io/book/#how-to-read-this-book). May also add notes from [The Rusty Web](https://davidmcneil.github.io/the-rusty-web/). 

* [Review](#review)
* [Book](#book)
* [The Rusty Web](#rustyweb)

## Reviewing JavaScript, HTML, CSS <a name="review"></a>

> [Mozilla Docs](https://developer.mozilla.org/en-US/docs/Learn)

## Rusty WASM Book <a name+"book"></a>

> [Setup](https://rustwasm.github.io/book/game-of-life/setup.html)

We use ```wasm-pack``` to orchestrate the following:
* Ensure that we have Rust 1.30 (or newer) and the ```wasm32-unknown-unknown``` target installed via ```rustup```
* Compile Rust sources into a WebAssembly ```.wasm``` binary via ```cargo```
* Use ```wasm-bindgen``` to generate the JavaScript API for using Rust-generated WebAssembly

To do this, we can execute the following command in our project directory (note that there are specific dependencies in the initial Rust cargo project):
```
wasm-pack build
```
This puts the artifacts in the ```pkg``` directory with the following contents:
```
pkg/
├── package.json
├── README.md
├── wasm_game_of_life_bg.wasm
├── wasm_game_of_life.d.ts
└── wasm_game_of_life.js
```
The ```.wasm``` file is the WebAssembly binary that is generated by the Rust compiler from our Rust sources. 

The ```.js``` file is generated by ```wasm-bindgen``` and contains JavaScript glue for importing DOM and JavaScript functions into Rust. It exposes a nice API to the WebAssembly functions to JavaScript. 

The ```.d.ts``` file contains TypeScript type declarations for the JavaScript glue. If we don't use TypeScript, we can ignore this file.

The ```package.json``` file contains metadata about the generated JavaScript and WebAssembly package. This is used by npm and JavaScript bundlers to determine dependencies across packages, package names, versions, and a bunch of other stuff. It helps us integrate with JavaScript tooling and allows us to publish our package to npm.

In order to take our package and use it in a web page, we can use the ```create-wasm-app``` JavaScript package template. In our main directory, we can run the command:
```
npm init wasm-app www
```
This creates a new subdirectory with the following content:

```
wasm-game-of-life/www/
├── bootstrap.js
├── index.html
├── index.js
├── LICENSE-APACHE
├── LICENSE-MIT
├── package.json
├── README.md
└── webpack.config.js
```
In this subdirectory, the ```package.json``` comes pre-configured with ```webpack``` and ```webpack-dev-server``` dependencies.

The ```index.js``` file is the main entry point for our Web page's JavaScript. 

The next step is to ensure the local development server and its dependencies are installed by running ```npm install``` in the ```www/``` subdirectory.

Next, we want to run ```npm link``` inside the ```pkg/``` subdirectory so that the local package can be depended upon by other local packages without publishing them to npm.

Next, we use the ```npm link```ed version from ```www``` package by running the following command in the ```www/``` subdirectory:
```
npm link wasm-game-of-life
```

Within ```www/```, we run ```npm run start``` in another terminal and the site will be served at ```htttp://localhost:8080/```.

### Interfacing Rust and JavaScript
JavaScript's garbage-collected heap is distinct from WebAssembly's linear memory space (which is where the Rust values live). Although WebAssembly currently has no direct access to the garbage-collected heap, JavaScript can read and write to the WebAssembly linear memory space (but only as an ```ArrayBuffer```); likewise, WebAssembly functions also take and return scalar values. 

```wasm-bindgen``` defines a common understanding of how to work with compound structures across the Rust-WASM-JavaScript communication paradigm. This entails boxing Rust structures and wrapping the pointer in a JavaScript class for usability. The ```wasm-bindgen``` crate is useful but does not necessarily remove the need for considering our data representation.

When designing an interface between WASM and JavaScript, we want to optimize the following properties:
1. Minimizing copying into and out of the WebAssembly linear memory
2. Minimizing serializing and deserializing

> In general, a good JavaScript--WASM interface design entails architecting large, long-lived data structures implemented as Rust types that live in WASM linear memory and are exposed to JavaScript via opaque handles.

JavaScript calls exported WASM functions and takes the opaque handles, transforms the data, performs heavy computations, queries the data, and returns a small, copy-able result. By only returning the small result of computation, we avoid copying and/or serializing everything back and forth between the JavaScript garbage-collected heap and the WASM linear memory.

> [relevant crates](https://rustwasm.github.io/book/reference/crates.html)
> [relevant tools](https://rustwasm.github.io/book/reference/tools.html)
> [templates](https://rustwasm.github.io/book/reference/project-templates.html)


## The Rusty Web <a name="rustyweb"></a>

**Rust**<br>
* systems programming language which can be compiled to various targets (two of which include asm.js and WebAssembly)
* blazingly fast, prevents segfaults, and guarantees thread safety

**WebAssembly (WASM)**<br>
* low-level programming language for in-browser client-side scripting 
* portable stack machine (designed to be faster to parse than JavaScript and faster to execute)
* designed for compilation to the web from low-level programming languages

**asm.js**<br>
* intermediate programming language designed to allow computer software written in languages such as C to be run as web applications while maintaining performance characteristics considerably better than JavaScript
* strict subset of JavaScript

**Emscripten**<br>
* source-to-source compiler that runs as a back end to the LLVM compiler to produce a subset of JavaScript known as asm.js (and WASM)

*The Rust compiler, using Emscripten as a backend, compiles Rust code into asm.js and WebAssembly which can be run by a browser.*