---
published: true
layout: post
title: Nebulet - Booting Up
---

This summer, I'm working on a GSoC project for Mozilla, creating [Nebulet](https://github.com/nebulet/nebulet), a microkernel that executes WebAssembly modules instead of compiled machine-code. Furthermore, it does so in ring 0 and in the same address space as the kernel. Normally, this would be super dangerous, but WebAssembly is designed to run safely on remote computers, so it can be securely sandboxed without losing performance.

Here's the beginning of the actual project proposal:

> To design and partially implement a research operating system that uses WebAssembly to implement software- isolated processes (SIPs) that run in ring 0 in order to allow architecture-agnostic compiled binaries and possibly attain higher performance due to not needing page-table switches and interrupt-based system-calls. The general idea is inspired by Singularity OS, a Microsoft research project from the mid-nauts.

I got a head start on working on the project and it accumulated a lot of interest even before the project was accepted.

This is intended to be the first of a (hopefully) weekly series of blog posts about Nebulet.

Without further ado, let us begin:

### Inspiration

When talking about software-isolation in contrast to hardware-isolation, it'd be unfair to not mention [Singularity OS](https://en.wikipedia.org/wiki/Singularity_(operating_system)). In short, Singularity is pretty similar to the concept behind Nebulet, but it was designed by a research group at Microsoft in the mid 2000s and all applications (including drivers and most of the kernel itself) are written in a variant of C#.

Nebulet is definitely inspired by Singularity.

### Where is it right now?

Right now, at the beginning of the coding phase of GSoC, Nebulet already has a preemptive scheduler and integrates with [Cretonne](https://github.com/cretonne/cretonne) to compile wasm directly to x86_64 machine code at runtime. I recently got the external function calling working, so wasm modules can "syscall", except, since it's all in a single address space, it's just a far call.

Currently, I'm working on statically regulating access to system resources. For example, a driver (which is also just a SIP) can specify that it will access a certain I/O port. The permissions get checked at compile-time and if they're insufficient, the binary will get rejected.

![Abi Interface]({{site.baseurl}}/images/wasm_abi_interface.png)

### Conclusion

Nebulet is just a way to try new ideas in operating system design. It's a playground for interesting and exotic techniques that could improve performance, usability, and security.
