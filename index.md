---
layout: home 
---

<img src="https://github.com/UoB-HPC/BabelStream/blob/gh-pages/img/BabelStreamlogo.png?raw=true" alt="logo" height="300" align="right" />
BabelStream is a benchmark used to measure the memory transfer rates to/from capacity memory.
Unlike other memory bandwidth benchmarks this does *not* include any PCIe transfer time for attached devices.
This benchmark is similar in spirit, and based on, the STREAM benchmark [1] for CPUs.

The choice of one programming model over another should ideally not limit the performance that can be achieved on a device.
As such there are multiple implementations in a variety of programming models.
Currently implemented are:

  - OpenCL
  - CUDA
  - OpenACC
  - OpenMP 3 and 4.5
  - Kokkos
  - RAJA
  - SYCL

As such this tool can be used as a kind of **Rosetta Stone** which provides both a cross-platform and cross-programming model array of results of achievable memory bandwidth.

[1]: McCalpin, John D., 1995: "Memory Bandwidth and Machine Balance in Current High Performance Computers", IEEE Computer Society Technical Committee on Computer Architecture (TCCA) Newsletter, December 1995.

## How is this different to STREAM?

BabelStream implements the four main kernels of the STREAM benchmark (along with a dot product), but by utilising different programming models expands the platforms which the code can run beyond CPUs.

The key differences from STREAM are that:
* the arrays are allocated on the heap
* the problem size is unknown at compile time
* wider platform and programming model support

With stack arrays of known size at compile time, the compiler is able to align data and issue optimal instructions (such as non-temporal stores, remove peel/remainder vectorisation loops, etc.).
But this information is not typically available in real HPC codes today, where the problem size is read from the user at runtime.

BabelStream therefore provides a measure of what memory bandwidth performance can be attained (by a particular programming model) if you follow today's best parallel programming best practice.



## Download

The source code is available on [GitHub](https://github.com/UoB-HPC/BabelStream)

## Run rules

In order to generate a valid BabelStream result, you should obey the following rules:

1. The array size must be large enough that increasing the array size does not drastically change the reported bandwidth. If you satisfy this you will no longer be affected by any cache effects.

2. Each kernel should take a few milliseconds. The resolution of the host timer is probably in milliseconds so each kernel execution should be longer than the timer resolution. Increase the array size if the kernels are too fast. You are unlikely to break this rule if you follow rule 1.

If you see a single BabelStream result, expect this to be the largest bandwidth reported by the benchmark unless stated otherwise.


