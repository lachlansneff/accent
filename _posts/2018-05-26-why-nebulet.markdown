---
published: true
layout: post
title: Why Nebulet?
---

I've gotten a lot of questions, both recently and when I first unveiled that I was creating Nebulet, that, in short, asked why on earth was I making this a thing. This post explains why.

For the uninitiated, Nebulet is an operating system that runs all applications as WebAssembly that's compiled into machine code and executed. This has a number of advantages, some of which I explored in the first post and others which I will expand on later.

Without further ado, the reasons why Nebulet exists:

### I think it's cool.

Nebulet is, in isolation, just awesome. It combines all sorts of computer science catagories that interest me, like compilers, virtual machines, operating systems, distrbuted computing, and more.

### We can make better choices.

This os is a chance to combine these in ways that haven't been done before. We can throw out the old and replace it with new, better-thought-out substitutes. There is no pretence of posix compatability.

Nebulet is a microkernel, with ipc efficiency high on the list of priorities. By using wasm as the isa, instead of an instruction set for a specific cpu type, architecture incompatibilities are thrown out the window. A single compiled binary will run on any system that Nebulet supports at full-native speed.

### It's a way to try new things.

Operating system design has been somewhat stagnant since, well, ever. Sure, once in a while, you hear of a cool os that some company worked on ten years ago or an interesting prototype that has recently crawled it's way out of a professor's underground lab, but that's pretty much it as far as I can tell (read: my limited experience).

Nebulet lets us try ideas that have been left dormant again. In fact, since I first started talking about Nebulet, I've gotten at least 3 or 4 people say that they were facinated by Singularity OS from a decade ago and hope that Nebulet lets that concept take off.

## What's next?

I'm not sure yet. I've been working on building out the interface exposed to applications by Nebulet and tried running rust compiled into wasm in Nebulet earlier today (It didn't work)!

Leave your comments (and criticisms) below, join the gitter [chatroom](https://gitter.im/nebulet/nebulet), and check out [Nebulet](https://github.com/nebulet/nebulet)!
