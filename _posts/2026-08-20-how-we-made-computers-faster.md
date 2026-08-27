---
layout: post
title: "How We Made Computers Faster"
author: "Harin"
tags: ComputerArchitecture Hardware Parallelism
---

<br /><br />
![A timeline of CPU parallelism: single-cycle execution, pipelining, superscalar and out-of-order, SIMD, multicore, and SMT](/assets/how-we-made-computers-faster/cover.jpg)
<br /><br />

Computing history can be viewed as one long attempt to answer a simple question:

How do we keep the hardware busy?

In the beginning, a CPU was essentially a single worker. It had a register file, an ALU, and logic to fetch and decode instructions. It fetched an instruction, decoded it, executed it, and wrote the result back.

If we wanted more performance, the obvious answer was simple: increase the clock speed.

But we soon realized something inefficient was happening. While the ALU was executing, the fetch and decode logic was often sitting idle. While one instruction was being written back, other parts of the CPU had nothing to do.

So we split the work into stages and pipelined the processor.

Fetching one instruction, decoding another, executing a third, and writing back a fourth could now happen simultaneously. Like an assembly line, different parts of the CPU could work on different instructions during the same cycle.

But then came the next question:

Why have only one ALU?

Programs contain many instructions that are independent of one another. If one instruction is adding two numbers, another might be multiplying two completely unrelated numbers.

So we widened the machine.

Multiple execution units could operate simultaneously, while increasingly sophisticated hardware looked ahead, identified independent instructions, and scheduled them for execution. This gave us superscalar processors and out-of-order execution — extracting instruction-level parallelism directly from ordinary sequential code.

Then we discovered another kind of parallelism.

A huge amount of computation involves performing the same operation on many pieces of data. Add these eight numbers to those eight numbers. Compare these sixteen pixels. Multiply these vectors.

Instead of executing eight separate instructions, why not execute one instruction across eight data elements?

That gave us SIMD — Single Instruction, Multiple Data.

Vector units allowed a single instruction to operate on multiple lanes of data simultaneously. The hardware was no longer just finding independent instructions; it was doing more work with each instruction.

But eventually, single-core scaling hit a wall.

The problem wasn't a lack of cleverness. It was power and heat.

Making one core increasingly complex and increasingly fast became too expensive in terms of power consumption. So instead of building one enormous core, we started putting multiple cores on the same chip — and multiple sockets in larger systems.

Parallelism moved up another level.

Now the programmer had to explicitly express more of the parallelism: threads, synchronization, shared memory, locks, and eventually sophisticated parallel programming models.

But there was still a problem.

Even with multiple cores and powerful execution units, those units frequently sat idle.

A cache miss could take hundreds of cycles. During that time, the CPU might have plenty of computational capacity but nothing useful to execute.

So we asked:

What if the core could work on something else while it waited?

Instead of adding another complete core, we duplicated enough architectural state — registers, program counters, and related state — to keep multiple threads in flight.

When one thread stalled waiting for memory, the processor could execute another.

This is SMT (Simultaneous Multithreading), commonly known as Hyper-Threading.

Crucially, SMT doesn't magically give the core another set of ALUs. It gives the existing execution resources more opportunities to stay busy.

The whole arc is one idea repeated at different scales: find the idle hardware, and find work to fill it.

And it never stopped — GPUs took SIMD to the extreme, and accelerators like TPUs and NPUs took it further still by hardwiring the one operation that matters most.
