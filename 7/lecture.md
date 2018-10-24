# Using Rust at Parity to Build Substrate to Build Polkadot (part 2)

This is the second part of a two part series on using Rust at Parity to Build Substrate to Build Polkadot. The first part can be found in lecture [6](../6/lecture.md).

* [WASM](#wasm)
* [Parity](#parity)

## WASM <a name ="wasm"></a>

* [A cartoon intro to WebAssembly](https://hacks.mozilla.org/2017/02/a-cartoon-intro-to-webassembly/)
* [What is WebAssembly](https://developer.mozilla.org/en-US/docs/Learn)

* [What makes WebAssembly fast?](https://hacks.mozilla.org/2017/02/what-makes-webassembly-fast/)
* [Creating and Working with WebAssembly modules](https://hacks.mozilla.org/2017/02/creating-and-working-with-webassembly-modules/)

## Parity <a name="parity"><a/>

* [Zero Knowledge Podcast](https://www.zeroknowledge.fm/46) -- recent interview with Gavin Would

> started as ETHcore team led by Gavin Would

> had ideas on how to build a heterogeneous sharding framework, but progress was too slow on Ethereum

> 99% fault tolerance article by Vitalik; we use multiple actors to increase fault tolerance; this isn't novel, it was already discovered by others

Four roles:
* validators
* nominators
* collators
* fishermen

Revised model:
* validators (&& nominators)
    -> sit on relay chain (central chain <-> beacon chain)
relay chain is like a neutral third party that keeps the parachain honest according to its protocol (security comes from relay chain)
* collators (&& fishermen)
    -> sit on parachain, looks for new blocks on parachain (a shard)

* [Why Rust](https://medium.com/paritytech/why-rust-846fd3320d3f)
* [Why We Need Web3.0](https://medium.com/@gavofyork/why-we-need-web-3-0-5da4f2bf95abs)
* [Substrate in a Nutshell](https://www.parity.io/substrate-in-a-nutshell/)