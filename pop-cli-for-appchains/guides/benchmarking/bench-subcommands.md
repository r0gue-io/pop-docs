---
description: Overview of Pop CLI benchmarking subcommands for block, machine, overhead, and storage.
---

# Benchmarking subcommands

Use these `pop bench` subcommands to profile different aspects of runtime and node performance. Each subcommand has its own flags and inputs, so use `--help` for details.

## Block

Benchmark block execution for a runtime.

```bash
pop bench block
```

## Machine

Benchmark machine performance and produce baseline data used by other benchmarks.

```bash
pop bench machine
```

## Overhead

Measure overhead costs for block processing.

```bash
pop bench overhead
```

## Storage

Benchmark storage read/write costs for a runtime.

```bash
pop bench storage
```

## Next steps

Run `pop bench <subcommand> --help` to see required flags and example inputs for your runtime.
