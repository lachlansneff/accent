---
published: true
layout: post
title: The End of the Beginning
---
Google Summer of Code is nearly done. I got a lot done this summer, but there is still a ways to go before Nebulet is usable in the real world!

--------

This post will specify what Nebulet is, why it exists, why GSoC was the perfect incubator for it, where it's going, and what my thoughts are about the whole experience.

## Project Overview

In short, Nebulet is an operating system that runs WebAssembly.

Going a little deeper, Nebulet is a microkernel that executes wasm in ring 0 for increased performance and the ability to do exotic optimizations that would not be possible on conventional operating systems. All the code aside from the kernel, which is about 10 thousand lines (excluding [cranelift](https://github.com/cranestation/cranelift), the wasm compiler), is compiled to webassembly. That includes drivers and all user-supplied code.

## The Original Proposal

To design and partially implement a research operating system that uses WebAssembly to implement software-isolated processes (SIPs) that run in ring 0 in order to allow architecture-agnostic compiled binaries and possibly attain higher performance due to not needing page-table switches and interrupt-based system-calls. The general idea is inspired by Singularity OS, a Microsoft research project from the mid-nauts.


The original goals that I set for this program were:

1. Integrate Cretonne (now called Cranelift)
2. Develop an ABI
3. Create the usermode

Of course, developing an operating system is a fluid thing and goals are subject to change over time, but I've completed all three of these for the time being.

## Why Nebulet Exists

Nebulet exists as a platform to test a different way that operating systems could've been developed. By using a language-based virtual machine to protect processes from eachother, one can do away with separate address spaces and interrupt-based system calls. All in all, this results in a simplier, and hopefully faster, operating system.

## Challenges

I can honestly say that the most challenging thing about this project was implementing threading support for Nebulet. Given that I had *very* little osdev experience before this, starting the project at all was pretty crazy, and diving straight into writing a fully-preemptive kernel was insanity. I've spent hours upon hours, days upon days, and weeks upon weeks staring at the file "thread.rs" and writing and rewriting it over and over again. I spent the first weeks of GSoC getting it to work and over the course of GSoC, I've probably spent 20% of the total time spent on Nebulet working on threading, and some parts, like lazy stack mapping, *still* don't work right.

Here are two examples of the communications during my mentor and I during these trying times:


![](https://i.imgur.com/Rd3w7kX.png)

------

![](https://i.imgur.com/8vZAIWO.png)

## Google Summer of Code

GSoC is really the perfect way to start projects like Nebulet. You are able to call upon the experience of your mentor and it helps build a community. My mentor, Yury Delendik, has been incredibly useful, as a motivator, an advisor, and collegue, and a friend. I with the best of luck to him and hope that he takes on the task of being a GSoC mentor again next summer.

The summer was split up into three parts, each with a blog post that specified the work done during that period.

The summer was started with [this post](https://lsneff.me/nebulet-booting-up.html), which told readers what Nebulet was and its original inspirations. This following time period was largely spent by adding support for the entire WebAssembly ISA. Cranelift, the wasm compiler, was, at this time, changing and upgrading quickly and it took a signifigant amount of time to keep up.

Quickly after that post, I added [another](https://lsneff.me/why-nebulet.html), detailing the reasons why Nebulet existed.

The end of the second work period was finalized with [this](https://lsneff.me/more-answers.html) blog post, which was essentially a FAQ about Nebulet and gave an example of program that could be written for Nebulet.

At this point in time, Nebulet had complete support for wasm. I was so excited, I tweeted about it:

![tweet](https://i.imgur.com/Y9MImlP.png)

After that, I spend the rest of GSoC working on writing the usermode (or, as I've taken to calling it, "sipspace"). So far, there is a vga text buffer driver and a ps2 keyboard driver. Additionally, during this time, Nebulet continued to build a community of people interested in contributing and watching the project grow.

The post you're reading now signifies the end of GSoC 2018.

Do not fear, for Nebulet will continue on!

## Where Nebulet is Going

I'm tentatively aiming Nebulet for use in servers. With the minimized system call overhead and potential for faster-than-native performance under certain workloads, Nebulet *could* be a more performant server os than linux.

For the fall, my work on Nebulet is going to be sponsored by Oasis Labs. The goals I have for that time period are:

* Handle 100mbps network load.
* Build infrastructure for caching compiled-code.
* Demonstrate wasm running in Nebulet on riscv64.

with a stretch goal of implementing multiprocessor support.

## Why GSoC is a Good Thing

Though the course of this summer, I learned that I could stick to a single project and see it out. I learned that, if I was determined, I could do anything I set my mind to.

Most of all, Google Summer of Code has cemented in my mind that open-source is important. Without open-source, Nebulet could not have happened for several reasons. The wasm compiler I'm using is licened with the MIT license, enabling Nebulet to use it. Rust, itself, the language I'm writing Nebulet in, is MIT licensed. But, more importantly, I wouldn't have met the all the interesting, cool people that I'm now friends with and I wouldn't have applied for GSoC in the first place.

Thanks you to all of you who have kept up with Nebulet over the past few months, to Mozilla for picking my proposal, and to Google, for supporting the open-source community! Your interest and ideas are very important to me (and it helps keep me motivated).
