---
description: Hash data quickly for identifiers, integrity checks, and tooling when building on Polkadot with Pop CLI.
---

# Hashing with Pop CLI

When you're building on Polkadot, you'll often
need [hashes](https://docs.polkadot.com/polkadot-protocol/parachain-basics/cryptography/#hash-functions) — to verify
files, derive stable identifiers, or work with chain primitives. The `pop hash` command gives you a fast, convenient way
to generate these hashes without juggling separate tools.

## What you can use it for

- Verify the integrity of artifacts (WASM, configs, datasets) before committing or deploying.
- Turn strings or hex into deterministic IDs for testing or scripting.
- Hash raw transaction/storage bytes when inspecting or debugging.
- Produce quick, non-cryptographic checksums during development loops.

## Quick examples

```bash
# Verify a string or small blob (cryptographic)
pop hash sha2 256 "hello world"

# Hash raw hex bytes (great for tx/storage inspection)
pop hash keccak 256 0x68656c6c6f776f726c64

# Check a runtime you are about to deploy
pop hash blake2 256 /path/to/your/file.wasm
```

## Tips

- Input can be plain text, 0x-prefixed hex, or a file path — Pop detects it for you.
- Choose the algorithm that fits the job:
    - SHA-2: dependable, broadly used.
    - BLAKE2: fast and secure (widely used in the Polkadot ecosystem).
    - Keccak: common in EVM tooling and ecosystems.
    - xxHash (TwoX): very fast, non-cryptographic for dev workflows.
- Pick an output length that matches your target (256-bit is a solid default; 512-bit for extra margin, 64/128-bit for
  speedier non-crypto checks).
