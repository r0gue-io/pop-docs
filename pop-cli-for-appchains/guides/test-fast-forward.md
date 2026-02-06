---
description: Fast-forward chain state with try-runtime using Pop CLI.
---

# Test fast-forward

Use `pop test fast-forward` to advance chain state with `try-runtime` and validate migrations across multiple blocks.

## Prerequisites

- A runtime built with the `--try-runtime` feature.
- Pop CLI installed with the `chain` feature.

## Usage

```bash
pop test fast-forward [OPTIONS]
```

## Example

```bash
pop test fast-forward --help
```

## Notes

Use `--help` to see the required inputs and available flags for your runtime and target chain.
