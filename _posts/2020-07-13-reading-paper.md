---
title: '[2020-07-13]Orbital Edge Computing: Nanosatellite Constellations as a New Class of Computing System'
date: 2020-07-13
permalink: /posts/2020-07-13/
tags:
  - reading paper
  - computer architecture
  - edge computing
---


This is another best paper in ASPLOS 2020. "I think it's the dawn of a new era in space exploration, I thinks a very exciting era," said Elon Musk. So I am impressed by the edging computing in space.

## Motivation

A massive nanostatellite constellations demands a new architecture for space systems. The system today still rely on communication that sends commands to orbit and delivers data to Earth, reffered as *bent pipe*. But the lack of scalability, high latencies and low data rate of bent pipe makes it less efficient for massive nanosatellite. This paper propose **Orbital Edge Computing (OEC)** to adderress the weakness of the current system.

Nanosatellite constellations are subject to a few constraints: 1) energy in space must be harvasted from the environment, so limited power due to the the small size 2) communication bitrate is affected by orbit parameters, ground stations capacity, etc.3) data quality is physically constrained. Based on the space study, this work proposes **computational nanosatellite pipelines(CNPs)** to hide latency.

## Design

CNPs divide image frames into tiles. There are four modes of operation for CNPs, divided in two dimensions - processing distribution and data distribution. *Frame-parallel*, where each frame is processed by different satellite, and *tile-parallel*, where each frame is processed by multiple satellites, describe how image processing tasks are distributed across a CNP. *Closed-spaced*, where all of the satellites have access to the same data at the same time, and *frame-spaced*, where different satellites have acees to different data at different time, describe the physical condifuraions of CNPs. 

![four modes](../../images/OEC_modes.png)


This work also provides a *cote* system as a full-system simulation of OEC systems. The detailed equations are shown in the paper. I won't talk much about this cause I am no expert ar voltage/ energy.


## Evaluation

The results of *cote* simulation shows that bent pipes are fundamentally unscalable. Freme-spaced is better because it put distance between satellites, reducing *downlink contention*. OEC makes constellations more scalable. The performance of OEC can be further enhanced by adopting *intelligent early discard*.

