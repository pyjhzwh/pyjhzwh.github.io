---
title: '[2020-07-26]High-Performance Deep-Learning Coprocessor Integrated into x86 SoC with Server-Class CPUs'
date: 2020-07-26
permalink: /posts/2020-07-26/
tags:
  - reading paper
  - computer architecture
  - DL coprocessor
---

This paper is an industry-track paper in ASPLOS 2020. I used to read more acedemic paper than industry paper, so I lack the knowledge what the focus of the industry. This paper decribes Centaur Technology's Ncore, the industry's first high-performance DL coprocessor technology in-target into an x86 SoC with server class CPUs.

Some important differences between industy paper and academic paper are that the industy-track paper is more "practical", more implementation details but not many "fancy" ideas. Looking at the design tradeoff options and decisions in this paper, it considers target environment, design optimization, connectivity, architecture type, voltage, supported data types. It is a good template for me to consider the design choices.

## Ncore Microarchitecture
Skipping the connectivity, memory details of Ncore, I will jump to Ncore architecture at the slice level. The execution pipeline comprises the neural data unit(NDU), neural processing unit(NPU), and output unit(OUT).

The instructions in Ncore are 128bit wide, which is similar to VLIW. A loop code can be encoded into a single instrucion in Ncore and execute in one single cycle.

The NDU performs data bypass, data row rotation, data block compression, byte broadcasting, and masked merge of input with output. It typically performs 2 of these operations in parallel, wihin a single clock cycle. Besides, Each NDU is connected to neighboring NDU slice, allowing roatation of data across slice boundaries.

In NPU, the data inputs can be forwared to the adjacent slice's NPU, allowing full-width data forwading.

NPU enables three configurable debug features: event logging, performance counter, and n-step breakpointing.

## Software Design
Ncore's software stack integrates with TensorFlow-Lite through the Delegate interface. The Ncore Graph Compiler Library (GCL) provides frontends to import frame-specific graph intermediate representation(GIR). Then graph optimization will be applied, producing an optimzied binary to run on Ncore. Despite generic optimizations, there are some hand-tuned inner kernels written in Ncore Kernel Library(NKL).

Ncore is connected to SoC via PCI. So the kernel driver unitlizes Linux's robus PCI envirnoment with little Ncore-specific tasks.

## Evaluation

In MLPerf's Inference v0.5 benchmarks, Ncore achieves the lowest latency in MobileNet-V1 and ResNet-50-V1.5, compared with other systems including NVIDIA AGX Xavier, Qualcomm SDM 855 QRD. Ncore yields 23x speedup over other x86 vendor per- core throughput, while freeing its own x86 cores for other work.