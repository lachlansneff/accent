---
published: true
layout: post
title: More answers.
---

Over the last couple of weeks, Nebulet has progressed signifigantly. Because of that, I think it's time to talk about why I made certain decisions when designing and writing Nebulet.

### Why Webassembly?

There are a couple reasons for picking webassembly over another isa, custom or otherwise. The first, and probably the most important, is that many languages can already compile into wasm. If you can write an application in c, c++, rust, go, or any other of the numerous languages that can already compile to wasm, or even compile an already existing application in one of those languages into wasm and run it with few to no changes, you'd be much more likely to do so.

Another reason is that webassembly can be quite performant.

### Why run wasm in ring 0?

Frankly, running wasm in ring 0 is probably dangerous. If anything (literally anything) goes wrong, the whole system will crash painfully and without warning. Okay, that's not quite true. The kernel will catch things like invalid opcodes and (most) out-of-bounds memory accesses. Of course, those things aren't supposed to happen and it's a bug if they do, but bugs happen.

The whole reason why I decided to run wasm in ring 0 is because of performance (and simplicity). You gain a couple of things when running everything in one address space:

1. System calls are just function calls, which have *much* less overhead than an interrupt or even `syscall` based syscall interface.
2. A single address space *may* have cache benefits. This has yet to be seen.

Since there are noteworthy benefits to running wasm in ring 0, the next question is...

### Why a new kernel?

I've been asked before why I decided to write a whole new operating system, kernel included, for Nebulet. There would be a couple of approaches if I had decided to build on a pre-existing kernel:

1. Use the Redox kernel and write a webassembly usermode.
2. Write a linux kernel module that runs webassembly.

The first was quickly decided against. While an important step for Rust, Redox is not a particularly good example of operating system design. More imortantly, modifiying it to include a compiler and all the necessary equipment would be a lot of work and the end-result would be messy and unstable.

The second hadn't actually crossed my mind and only came to my attention when I saw that someone else (another high-school student interestingly enough!) was following it for a [project](https://github.com/cervus-v/cervus) of theirs. It's a very cool idea and builds on the success of linux, but it may end of being too restrictive to implement a performant wasm runtime.

### Why Rust?

This one is very simple. I like Rust.

### More stuff

Nebulet is very close to running normal applications written in rust (though you could run anything else that compiled to webassembly, but I haven't written a library to interface with nebulet for those yet). It can do simple things, but still needs to be expanded before I can start seriously writing drivers for it.

Here's an example of a program that runs on nebulet:

```rust
#![no_main]

#[macro_use]
extern crate sip;

use sip::thread;
use sip::Mutex;

#[derive(Debug)]
struct Philosopher {
    name: &'static str,
    left: usize,
    right: usize,
}

impl Philosopher {
    fn new(name: &'static str, left: usize, right: usize) -> Philosopher {
        Philosopher {
            name,
            left,
            right,
        }
    }

    fn eat(&self, table: &[Mutex<()>]) {
        let _left = table[self.left].lock();
        let _right = table[self.right].lock();

        println!("{} is eating.", self.name);

        thread::yield_now();

        println!("{} is done eating.", self.name);
    }
}

#[no_mangle]
pub fn main() {
    use std::panic;

    panic::set_hook(Box::new(|info| {
        println!("{}", info);
    }));

    static TABLE: [Mutex<()>; 5] = [
        Mutex::new(()),
        Mutex::new(()),
        Mutex::new(()),
        Mutex::new(()),
        Mutex::new(()),
    ];

    let philosophers = vec![
        Philosopher::new("Judith Butler", 0, 1),
        Philosopher::new("Gilles Deleuze", 1, 2),
        Philosopher::new("Karl Marx", 2, 3),
        Philosopher::new("Emma Goldman", 3, 4),
        Philosopher::new("Michel Foucault", 0, 4),
    ];

    let threads: Vec<_> = philosophers.into_iter().map(|p| {
        thread::spawn(move || {
            p.eat(&TABLE);
        }).unwrap()
    }).collect();

    for thread in threads {
        thread.join().unwrap();
    }
}
```

This is an implementation of the dining philosphers problem and mostly taken from the rust book with adaptations for nebulet.

I'll explain some of the more interesting parts of this:

1. Threads.
    - Wasm doesn't have support for threading off the bat, so threads are implemented like they would be in a normal operating system. So, to create a thread, you call an abi function with the correct function table index as a parameter.
2. Mutexes.
    - Mutexes aren't natively supported either (though their building blocks may be part of the spec when wasm is expanded in the future). A mutex in nebulet is built on something that I'm calling a `pfex`, which stands for "pretty-fast exclusion". This is generally pretty similar to a futex in linux, but doesn't have a user-mode component, since it would require atomics, which aren't supported in wasm quite yet.

If you go ahead and clone down the nebulet repo and run `bootimage build --release -- -serial stdio` (that last part isn't strictly necessary, but it does route output to the terminal instead of just the popup vga window), this program will run. Feel free to experiment! Nebulet is in a state of flux right now, so there's never been a better time to try it out!

The Nebulet github repo is https://github.com/nebulet/nebulet.
