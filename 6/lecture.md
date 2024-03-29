# Rust

This lecture may need to be split into two parts. First, I want to introduce the Rust programming language. Then, I want to discuss the Parity team, the Substrate blockchain framework, and the Polkadot network (this was moved to next week's lecture [7](../7/lecture.md)).

* [Why I Love Rust](#rust)
    * [Memory Management](#safety)
    * [Culture](#culture)

## Why I Love Rust <a name="rust"></a>

* [Using Rust for an Undergraduate OS Course](http://rust-class.org/pages/using-rust-for-an-undergraduate-os-course.html) by Professor [David Evans](https://www.cs.virginia.edu/~evans/) (the supervisor of this course!)
> "C may be the language of operating systems today (and since the 1970s), but it is also the language of buffer overflow vulnerabilities, dangling pointers, double frees, memory leaks, undefined behavior, and race conditions. To me, it would seem inconscionable to have students use C in a way that is likely to lead to programs riddled with security vulnerabilities and robustness problems. On the other hand, actually teaching students to use C in a responsible way would probably require at least the full semester as well as use of many other tools1, including some that are only available commercially."

> "As far as I could tell, none of the OS courses that use C had any time for teaching students how to write reasonably safe C code (and none of the C-based OS textbooks I looked at made anything beyond superficial attempts to address safe programming practices in C), which is understandable given how much time it takes to learn the other topics in an OS class, but from my perspective, seems reckless and irresponsible. A C programmer who knows how to hack on an OS kernel, but doesn't know how to avoid memory leaks, double frees, and undefined behavior, should not be unleashed on an unsuspecting code base!" 

* [The Success of Go heralds that of Rust](https://medium.com/@george3d6/the-success-of-go-heralds-that-of-rust-73cb2e4c0500) -- October 13, 2018
* [fireflowers](https://brson.github.io/fireflowers/)
* [Julia Evans: Why I <3 Rust](https://jvns.ca/blog/2016/01/10/why-i-rust/)
* [Rust Basics](https://medium.com/learning-rust/rust-basics-e73304ab35c7)
* [Rust: The Tough Part](https://medium.com/learning-rust/rust-the-tough-part-2ea11ed3693e)

> The following description of *how* to go about learning Rust is taken from [thePGoat's repo](https://github.com/thePGoat/madamePsychosis)

Start with [The Book](https://doc.rust-lang.org/book/), then [The Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/). Consider building a small personal project while you're learning the language -- this preferrably should not be something *cutting edge*. It is better to start with something small and iteratively add to it instead of setting lofty, unrealistic goals from the start. 

* [Rust Error Handling Intro](https://brson.github.io/2016/11/30/starting-with-error-chain)
* [Steve Donovan's Gentle Introduction to Rust](https://stevedonovan.github.io/rust-gentle-intro/readme.html)
* [Tutorials (may be outdated)](http://aml3.github.io/RustTutorial/html/toc.html)
* [Recent Stanford OS Class](https://web.stanford.edu/class/cs140e/syllabus/#schedule)
* [Some other OS Class](http://ozark.hendrix.edu/~ferrer/courses/os/)

> I've organized my own Rust notes [here](https://github.com/AmarRSingh/CS1501/tree/master/Tracks/AppDev/Rust).

### Memory Management <a name="safe"><a/>

* [Excerpts From: Rust Types for Solid Safety](https://web.stanford.edu/class/cs140e/notes/lec2/paper.pdf)
* [Rust Static Garbage Collection](https://words.steveklabnik.com/borrow-checking-escape-analysis-and-the-generational-hypothesis)

### Culture <a name="culture"></a>

* [Rust's Code of Conduct](https://www.rust-lang.org/en-US/conduct.html)
* [aturon.log: listening and trust, part 2](http://aturon.github.io/2018/06/02/listening-part-2/)
