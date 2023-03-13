---
layout: post
title: Some thinking about concurrent programming
date: 2023-02-24 10:00:00
description: async and await, thread in Rust.
tags: Rust
categories: Rust, programming
giscus_comments: true
---

In last two weeks, I started to reuse Rust language, the dedicated and a little bit complicated tool with enormous magic. Actually, I admire Rust's philosophy and vision a lot, which concerns safety and efficiency and tries to strke a subtle balance between simplicity and power. 

One of the most intriguing parts of Rust is asynchronous programming, or async in short. The keywords `async/await` are not original invention of Rust, an increasing number of programming languages are supporting a concurrent programming model. It lets us run a large number of concurrent tasks on a small number of OS threads, while preserving much of the look and feel of ordinary synchronous programming. 

In this post, I am going to sumarize some reading and thinking about async recently.

## 1. Async vs other concurrency models

Here is a brief overview of the most popular concurrency models can help us understand how asynchronous programming fits within the broader field of concurrent programming:

+ **OS threads** don't require any changes to the programming model, which makes it very easy to express concurrency. However, synchronizing between threads can be difficult, and the performance overhead is large. Thread pools can mitigate some of these costs, but not enough to support massive IO-bound workloads.
+ **Event-driven programming**, in conjunction with *callbacks*, can be very performant, but tends to result in a verbose, "non-linear" control flow. Data flow and error propagation is often hard to follow.
+ **Coroutines**, like threads, don't require changes to the programming model, which makes them easy to use. Like async, they can also support a large number of tasks. However, they abstract away low-level details that are important for systems programming and custom runtime implementors.
+ **The actor model** divides all concurrent computation into units called actors, which communicate through fallible message passing, much like in distributed systems. The actor model can be efficiently implemented, but it leaves many practical issues unanswered, such as flow control and retry logic.
+ **async/await** has high efficiency and support low-level custom implementors with the ability to support normal programming model. However, innner implementation of async/await is tricky and comlicated which is hard to understand for normal users, compared with threads and coroutines. 

The goods news is Rust maintainers and the community has devoted greate energy in building an easy-used concurrency model for us. It consists of two parts:

+ **Multi-thread standard library** provides the basic ability to utilize system threads, which is very straightforward and clear.
+ **Async/await** are supported by language features + STL + third-part library. 

**CPU bound and I/O bound**

A program is **CPU bound** if it would go faster if the CPU were faster, i.e. it spends the majority of its time simply using the CPU (doing calculations). **A program that computes new digits of π** will typically be CPU-bound, it's just crunching numbers. We also call CPU-bound tasks as computing-intensive tasks. Actually, we need to utilize the advantage of **parallel programming** to address cpu-bound tasks. When we have to get into massive calculations, it is important to know how to divide the job and get it done by more than one of available cores at once, **in parallel**.

A program is **I/O bound** if it would go faster if the I/O subsystem was faster. Which exact I/O system is meant can vary; I typically associate it with the disk, but of course, networking or communication, in general, is common too. **A program that looks through a huge file for some data might become I/O bound since the bottleneck is then the reading of the data from disk** (actually, this example is perhaps kind of old-fashioned these days with hundreds of MB/s coming in from SSDs).

So, we need to choose different solutions while encoutering different tasks.

- `async` models are preferred with I/O bound tasks.

- If the task consist of IO bound and CPU bound tasks, we choose multiple threads. In most cases, a thread-pool is necessary to reduce the overhead of construction and release of native thread.

- If CPU bound tasks occupies a larger part, like parallel computing, a multi-thread model is preferred and the number of threads should be a little bit more than CPU cores.

- If there are not any specific restrictions, use threads to reduce runtime overhead.

  