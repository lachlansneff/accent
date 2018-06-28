---
published: true
layout: post
title: A Whole New World
---

Our current software infrastructure is, depending on how you see it, more than 25 years old. Obviously, very little of that original code from 1991 and earlier is still in the linux kernel, but the design is similar and the ideas are similar.

Maybe it's time for a redesign... of everything.

## Why

We face a couple of challenges with computers as we speed towards 2020 and beyond.

### 1. Escape to the browser

Nowadays, many, if not most, of the applications that developers use are literally js, html, and cs packaged up with Chromium in the form of Electron. I probably have at least 4 or 5 instances of Chromium on my laptop right now in the form of text editors, devRant, and Slack. And that ignores node.js, which uses the V8 javascript engine *from* Chromium!

Why on earth are developers fleeing from native GUIs like peasants from the plague?

The answer is simple: Native GUIs suck. Even with the boilerplate that comes along with html, it's still easier and more convienient to make a functional user interface.

### 2. Fragmentation

It's often difficult to get the same code to run on more than on OS. And this isn't even an issue of hardware. It's just software being purposely designed to be incompatable.

### 3. Mobile

More and more of all the code being written is consumed by mobile users. 

The problem? 

Programming for mobile sucks. And, unless you use a framework like ReactNative, you need completely seperate codebased for iOS and Android Apps (and don't even talk about Windows Phone).

### 4. Performance

Since Moore's Law has apparently slowed down signifigantly (maybe even stopped :/), people will turn to software developers for their yearly performance boost (by way of soft-optimizations, instead of hard ones).

### 5. Javscript

Javascript has a lot of problems (probably one of the largest being weak types), and since so many projects these days are written in javascript... It's impossible to escape it: Node.js on the server, regular js in the browser, Electron on the desktop. It's everywhere! You get the idea.

## WebAssembly is our savior

Now, you can write code for the web in any language? Hell yeah! Oh, and it's faster too. (At least for loading.)

We've all heard too much about WebAssembly over the last few days (Especially thanks to Steve), so that's not what this post is about.

## So is Rust

Safe, fast, and possible to use for every part of the full stack, from the kernel to the webpage. What's not to like?

## A new opportunity

With WebAssembly, we have the opportunity to take a long hard look at the entirety of our current infrastructure. I think it's time to take that a bit further.

We can solve the vast majority of the problems facing developers now and in the future in one fell swoop.

## A New OS for a New Future

What if we took all of these things and combined them? Can we get better performance, better interopobility, and a better, simplier ecosystem?

What if we, the collective developer community, decided that it's time for a refactor of *everything*? 

Picture this: a kernel that only ran WebAssembly. Drivers, applications, and everything else is compiled into WebAssembly (The core kernel would *obviously* be written in Rust).

I know, it sound ridiculous. Just hear me out.

### Automagically better security 

If everything runs in a VM, than code won't be able to do anything that the kernel doesn't want it to. No memory corruption bugs, no root exploits, no malware.

The code that isn't running in a VM would be written in Rust, so it would be safe too.

### Possibility of being signifigantly faster

If everything runs in the kernel, there's no need for systemcalls or context switches, both of which have signifigant overhead.

Even with the overhead of a vm (and WebAssembly can run at *nearly* native speed), this OS could be signifigantly (up to ~20%, if the findings of SingularityOS are to be believed) faster than current OSes on the same hardware.

### Everything works everywhere

If everything is written in WebAssembly, compatability is now a mute issue. The same code would work on **any** system, from arm microprocessors to supercomputers.

### A Better GUI

This part is more tentative, but it would be possible to use the DOM as a GUI primative. It might be slower (but nowadays, the DOM has been ridicuously optimized, so not much slower), but it'd definetly be easier and more pleasant to use. People could also use the same codebase (minus the javascript) for webapps and "native" apps.

## Conclusion

I think that we need to have a talk about the future of software and what we want software development to look like in 10, 20, or 30 years.

Let's write a new future that's better for all developers.

I've already started a Proof-Of-Concept for this project [here](https://github.com/lachlansneff/Metal). Let me know what you think on [Reddit](https://www.reddit.com/r/programming/comments/7g4nz9/a_whole_new_world/) or on Twitter.
