# Bullshark

[![build status](https://img.shields.io/github/actions/workflow/status/asonnino/narwhal/rust.yml?branch=bullshark&logo=github&style=flat-square)](https://github.com/asonnino/narwhal/actions)
[![rustc](https://img.shields.io/badge/rustc-1.51+-blue?style=flat-square&logo=rust)](https://www.rust-lang.org)
[![python](https://img.shields.io/badge/python-3.9-blue?style=flat-square&logo=python&logoColor=white)](https://www.python.org/downloads/release/python-390/)
[![license](https://img.shields.io/badge/license-Apache-blue.svg?style=flat-square)](LICENSE)

This repo provides an implementation of [Bullshark](https://arxiv.org/pdf/2201.05677.pdf). The codebase has been designed to be small, efficient, and easy to benchmark and modify. It has not been designed to run in production but uses real cryptography ([dalek](https://doc.dalek.rs/ed25519_dalek)), networking ([tokio](https://docs.rs/tokio)), and storage ([rocksdb](https://docs.rs/rocksdb)).

## Quick Start

The core protocols are written in Rust, but all benchmarking scripts are written in Python and run with [Fabric](http://www.fabfile.org/).
To deploy and benchmark a testbed of 4 nodes on your local machine, clone the repo and install the python dependencies:

```
$ git clone https://github.com/asonnino/narwhal.git
$ cd narwhal/benchmark
$ pip install -r requirements.txt
```

You also need to install Clang (required by rocksdb) and [tmux](https://linuxize.com/post/getting-started-with-tmux/#installing-tmux) (which runs all nodes and clients in the background). Finally, run a local benchmark using fabric:

```
$ fab local
```

This command may take a long time the first time you run it (compiling rust code in `release` mode may be slow) and you can customize a number of benchmark parameters in `fabfile.py`. When the benchmark terminates, it displays a summary of the execution similarly to the one below.

```
-----------------------------------------
 SUMMARY:
-----------------------------------------
 + CONFIG:
 Faults: 0 node(s)
 Committee size: 4 node(s)
 Worker(s) per node: 1 worker(s)
 Collocate primary and workers: True
 Input rate: 50,000 tx/s
 Transaction size: 512 B
 Execution time: 19 s

 Header size: 1,000 B
 Max header delay: 1_000 ms
 GC depth: 50 round(s)
 Sync retry delay: 10,000 ms
 Sync retry nodes: 3 node(s)
 batch size: 500,000 B
 Max batch delay: 100 ms

 + RESULTS:
 Consensus TPS: 46,478 tx/s
 Consensus BPS: 23,796,531 B/s
 Consensus latency: 464 ms

 End-to-end TPS: 46,149 tx/s
 End-to-end BPS: 23,628,541 B/s
 End-to-end latency: 557 ms
-----------------------------------------
```

## Next Steps

The next step is to read the paper [Bullshark](https://arxiv.org/pdf/2201.05677.pdf). It is then recommended to have a look at the README files of the [worker](https://github.com/asonnino/narwhal/tree/bullshark/worker) and [primary](https://github.com/asonnino/narwhal/tree/bullshark/primary) crates. An additional resource to better understand the Bullshark consensus protocol is the paper [Narwhal and Tusk](https://arxiv.org/pdf/2105.11827.pdf) describing the main systems aspects behind this protocol.

The README file of the [benchmark folder](https://github.com/asonnino/narwhal/tree/bullshark/benchmark) explains how to benchmark the codebase and read benchmarks' results. It also provides a step-by-step tutorial to run benchmarks on [Amazon Web Services (AWS)](https://aws.amazon.com) accross multiple data centers (WAN).

## License

This software is licensed as [Apache 2.0](LICENSE).
