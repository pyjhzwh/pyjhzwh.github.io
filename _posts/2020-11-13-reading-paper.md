---
title: '[2020-11-14] High-Performance Deep-Learning Coprocessor Integrated into x86 SoC with Server-Class CPUs'
date: 2020-11-14
permalink: /posts/2020-11-14/
tags:
  - reading paper
  - PIM
  - LUT
  - NN acceleration
---

This is a paper using processor-in-memory(PIM) for NN acceleration in ASPLOS 2020. I know little about PIM techinques. And I don't know much about the SRAM structure. I learned some basic knowledge of SRAM, PIM from this paper. And I learned a lot of the implementation tools and benchmark measuring tools.

## SRAM, PIM
* memory organization of SRAM: SRAM -> slices -> bank -> subbank-> subarray-> partition -> cells.
* Any PIM strategy which disturbs this tightly coupled sub-array organization will either negatively impact the data access (memory property) or increase the PIM execution latency.
* One of the most common PIM techniques is to assert multiple worlines where the input operands are located and establish a bitline discharge to obtain the computed output. Boolean operation takes 1 cycle. Limited to bitwise operations.
* Multiple row activation (MRA) will cause a write bias condition and my cause data corruption.
* Interconnect dominates the energy and latency of SRAM.
* PIM performance can be determined by the number of operations per PIM cycle (PIM-OPC). 

## Motivation
Most exsiting accelerators for DNN workloads has incorporates quantization, and using various bit precision for different layers. Such cases makes a better use case for LUT-based approaches.
Interconnect dominates the energy and latency of SRAM. So it is desirable to avoid data movement through the interconnect and compute within a subarray. And data parallelism can be enabled by concurrent processing at individual sub-arrays.
Integrating specialized hardware within each subarray will result in significant degradation of memory density and negatively impact normal memory operation. So BFree - a LUT based comptue that supports configurable for multiple operations.  

## BFree Architecture
![BFree architecture](../../images/BFree-LUT.png)
BFree architecture corporates compute capabilities at sub-array granularity in conventional cache memory organization.
Configuration block (CB) stores metadata.
BFree compute engine(BCE) incorporates three stage in-order pipeline: 1. Read instructions from CB and decode 2. Compute LUT addressed 3. Access LUT address and further accumulated/processed
Decoupled bitlines for the LUT rows alone to reduce access cost.
For reducing the number of LUT entries, they only store the entries for 4-bit operands in the sub-arrays. For higher precision operands, BCE will decompose the operands to 4-bit precision operands and accumulate the partial product. Further, the number of entries could be reduced to 49 by utilizing some fundamental multiplication properties - only storing the products if both the operands are odd numbers.
Other operations like division or activation functions can also be implemented with LUT-based approaches.

## Evaluations
Tools for BFree design: SPICE simulation for STAM, LUT and BCE design; Synposys DC and ICC compiler for auto place and route; Synopsys PrimeTime-PX for power evalutaion
Benchmarks: 1) In cache technique - Neural Cache 2) Eyeriss (Not sure how to compare Eyeriss energy/performance, does the author implement Eyeriss design?) 3) CPU, using Pytorch profiler, and use Intel Rapl tools tp measure power 4) GPU, using Tensorflow profiler, and using Nvidia-smi tools to measure GPU power
This PIM solution achieves 1.72x better performance while being 3.14x energy efficient compared to the state-of-the-art DNN in-memory accelerators when running inception v3. Our analysis show 101x, 3x speed up and 91x, 11x energy efficient than CPU and GPU



