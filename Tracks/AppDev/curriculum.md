# Application Development

Application Development is split up into a few different sections based on language. This document contains justifications for studying each language as well as resources for getting started in each language. 
* [Rust](#rust)
* [Go](#go)
* [JavaScript](#javascript)
* [Python](#python)

> Resources are grouped according to language in ```./language``` and each directory contains a ```schedule.md``` that provides a high level overview of the readings/themes for each week.

## Rust
Rust offers [A LOT](https://paritytech.io/why-rust/) of desirable features that makes it a very good programming language for blockchains. In addition, [Parity](https://www.parity.io/technologies/) is coding their software stack in Rust (both their blockchain clients and the [Polkadot network](https://polkadot.network)). For our purposes, we'll build Rust blockchain applications using Parity/Polkadot.

> Check out the [schedule](Rust/schedule.md) for more information

## Go
Go was designed by Google for Web servers and Networking with very good concurrency support. There are many useful libraries for blockchain interaction that are coded in Go. We'll focus on developing with both the [Loom Golang SDK](https://loomx.io/developers/docs/en/go-loom-clients.html) as well as [Tendermint](https://github.com/tendermint). Both of these clients offer scalability over many existing blockchain implementations.

> Check out the [schedule](Go/schedule.md) for more information

## JavaScript
> [The Birth & Death of JavaScript](https://www.destroyallsoftware.com/talks/the-birth-and-death-of-javascript)
JavaScript is still a very popular programming language, especially for front-end web applications. Despite its [humble](https://en.wikipedia.org/wiki/JavaScript) beginnings, JavaScript can be used for almost anything nowadays. For our purposes, we'll focus on developing with [AragonOS](https://github.com/aragon/aragonOS), but we may experiment with [other](https://github.com/counterfactual/contracts/blob/develop/ARCHITECTURE.md) libraries. In addition, we will probably also cover **Solidity, which has very similar syntax**.

> Check out the [schedule](JavaScript/schedule.md) for more information

## Python
There are many good [reasons](https://hackernoon.com/reasons-to-choose-python-for-ai-based-projects-7e3e6c8b954a) to code in Python. This track will emphasize data science and may focus on interacting with [BigChainDB](https://github.com/bigchaindb/bigchaindb). 

> Check out the [schedule](Python/schedule.md) for more information