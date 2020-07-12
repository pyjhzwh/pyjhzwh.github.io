---
title: '[2020-07-12]Challenging Sequential Bitstream Processing via Principled Bitwise Speculation'
date: 2020-07-12
permalink: /posts/2020/07/reading/
tags:
  - reading paper
  - computer architecture
---


This is one of the best papers in ASPLOS 2020. I like the idea of transforming a somewhat unfamiliar problem into a equivalent well-solved problem.

**Bitstream procesing** manipulates bitwise operation (like xor, shift) over long sequence of bits. But a fundamental challenge arises when the bits have dependencies between each other, referred as *bitstream-carried dependences*.

Previous efforts to optimize bitstream processing focus on fine-grained vectorization. But there are a few limitations. First, coding with SIMD intrinsics is diffucult and time-consuming. Second, this fine-grained optimization cannot be extended to coarse-grained level.

This work come up with an automatic-gernerated and coarse-grained level approach called *principled bitwise speculation (PBS)*. The bitstram program can be viewed as an analogy of sequential circuit. Then the depedencies can be modeled as *finite-state machines(FSMs)*. To minimize the number of states of FSM, the work first use static analysis to reason about the minimal depedent bits. Then contrust the FSM, treating the combination of dependent bits as *states*, examining input-output pairs to find the *transactions*. After modeling as FSM, many speculative FSM parallelization methods can be adopted. The work flow is shown as following.

![workflow](../images/workflow-bitstream.JPG)

The analysis process of depedent bits envoloves a lot techniques of compiler, like RAW, liveness analysis, CFG. And the *bit-status analysis* is addressed with techiques for hardware synthesis. So reading this paper makes me recall what I learned from EECS 583.

I have a few questions after reading this paper. In the Evalutation section, the depedent bits of the benchmark programs are suprisling low, ranging from 1 o 5 bits. I am not sure if these prgrams are quite small or the depedent bit analysis will reduce the bit numbers a lot. And I wonder the overhead of generating and storing FSM. It seemed that the overhead is not mentioned is this paper.

Overall, this paper is easy to read to understand, with the basic knowledge of FSM. The idea of viewing bitstream programs from a different perspective is brilliant.
