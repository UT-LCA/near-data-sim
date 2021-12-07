Virtual-Link
============

Virtual-Link (_VL_) is a novel light-weight communication mechanism with hardware support to facilitate M:N lock-free data movement.
_VL_ reduces the amount of coherent shared state, which is a bottleneck for many approaches, to zero.
_VL_ provides further latency benefit by keeping data on the fast path
(i.e., within the on-chip interconnect).
_VL_ enables directed cache-injection (stashing) between processing elements on the coherence bus, reducing the latency for core-to-core communication.
_VL_ is particularly effective for fine-grain tasks on streaming data.

This repository releases the code related to the Virtual-Link message queue architecture,
including the library, the benchmarks as well as the modified Gem5 simulator.

# Build

Clone the repository with submodules:
~~~
$ git clone --recurse-submodules https://github.com/UT-LCA/near-data-sim.git
~~~

## Library libvl

The library code has multiple build options for various of purposes.

To build without the Gem5 ISA extenstion support:
~~~
$ cd libvl && make NOSYSVL=true NOGEM5=true
~~~
Note: Virtual-Link is designed as an extension to Arm aarch64 architecture,
and the library code constains aarch64 assembly.
Please either build this on an aarch64 system (e.g., Raspberry Pi 4) or cross-compile.

## Benchmarks

Benchmarks are built by cmake (>=3.1).
Most of the benchmarks have `*_vl`, `*_boost`, and `*_zmq` variants,
hence requires libvl, [boost 1.63.0](https://www.boost.org/users/history/version_1_63_0.html),
as well as [zmq 4.2.1](https://github.com/zeromq/libzmq/tree/v4.2.1) are installed,
otherwise, some benchmark targets would be skipped for missing the dependent libraries.
~~~
$ mkdir -p VL_uBMK/build && cd VL_uBMK/build && cmake -DVL_ROOT=../libvl -DBOOST_ROOT=<boost install path> -DBOOST_LIBRARYDIR=<boost library path> -DZMQ_ROOT=<zmq install path> ../
~~~
Then build the benchmarks, `pingpong_vl` for example:
~~~
$ cd VL_uBMK/build && make pingpong_vl
~~~
