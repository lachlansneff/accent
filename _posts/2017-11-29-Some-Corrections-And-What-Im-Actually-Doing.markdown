---
published: true
layout: post
title: Some Corrections and What I'm Actually Doing
---

Sorry, my last (and first) post came out a little wrong. My intent wasn't to say, "let's make everything Electron." I really don't want that. And to be frank, I'm not really a GUI person. I use them, yes, but I don't much enjoy designing or creating them, whatever method is used.

The original intent--and I agree that I got off track--was to bring attention to the fact that we're still using software paradigms from the early 1990s and early. This isn't necessarily a bad thing, just that maybe over the last 25-30 years, we've come up with better ways of doing things.

My opinion is that managed kernels are that better way of doing things. Now, the majority of you automatically don't agree with me and if reddit is to be believed, some of you can "not continue past that point", but hear me out.

### WebAssembly

Wasm is pretty sweet. It can (eventually) run on anything. It's low-level enough to be a compile target for a lot of languages and it *may* be able to run at nearly native speed if AOT compiled (yeah, maybe SIMD makes a huge difference between compiled wasm and native, but whatever, wasm will get that eventually).

### Singularity OS

Singularity OS was a research project of Microsoft during the mid 2000s. It aimed to do everything that I think "this future OS" needs to do. The kernel is mostly managed code (C# in this case), processes were software-isolated, not hardware-isolated. Applications, drivers, and everything else ran as a process and communicated with a kickass IPC system.

And, the result? Singularity was signifigantly faster than Windows, OpenBSD, and Linux. While managed code incurs an overhead, it turned out that the gain in speed not using individual address spaces, hardware protection rings and interrupts as syscalls cancelled that overhead out and then some.

So, that's what I'm working on. Singularity OS but using WebAssembly and Rust. I don't plan on adding a gui or adding compatability for other architectures. It's just a hobby project, but I think that something like it, if only in spirit, should be developed.

Here's an extremely vague layout of the architecture:

![Diagram]({{site.baseurl}}/images/architecture.png)


