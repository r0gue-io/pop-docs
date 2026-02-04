---
description: Hash data quickly for identifiers, integrity checks, and tooling when building on Polkadot with Pop CLI.
---

# Hashing with Pop CLI

When you're building on Polkadot, you'll often
need [hashes](https://docs.polkadot.com/polkadot-protocol/parachain-basics/cryptography/#hash-functions) — to verify
files, derive stable identifiers, or work with chain primitives. The `pop hash` command gives you a fast, convenient way
to generate these hashes without juggling separate tools.

You can also run the command as `pop h`.

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

# Append the original bytes to the hash (BLAKE2 or TwoX only)
pop hash blake2 256 --concat "hello world"
```

## Supported algorithms and lengths

| Subcommand | Alias | Lengths (bits) | `--concat` |
| --- | --- | --- | --- |
| `blake2` | `b2` | 64, 128, 256, 512 | Yes |
| `keccak` | `kk` | 256, 512 | No |
| `sha2` | `s2` | 256 | No |
| `twox` | `xx` | 64, 128, 256 | Yes |

## Input handling

The positional `data` argument is inspected in this order:

- If it matches an existing path, Pop reads the file contents. The file must be 3 MiB or smaller.
- If it starts with `0x`, Pop parses it as hex bytes.
- Otherwise, Pop hashes the raw string bytes.

## Output

Pop prints the hash as lowercase hex without a `0x` prefix. If you use `--concat`, the output is the hash bytes followed by the original input bytes, all hex-encoded.

## Errors and constraints

- Unsupported length: `unsupported length: <length>`
- Path exists but is not a file: `specified path is not a file`
- File too large: `file size exceeds maximum code size`

## Tips

- Input can be plain text, 0x-prefixed hex, or a file path — Pop detects it for you.
- Choose the algorithm that fits the job:
    - SHA-2: dependable, broadly used.
    - BLAKE2: fast and secure (widely used in the Polkadot ecosystem).
    - Keccak: common in EVM tooling and ecosystems.
    - xxHash (TwoX): very fast, non-cryptographic for dev workflows.
- Pick an output length that matches your target (256-bit is a solid default; 512-bit for extra margin, 64/128-bit for
  speedier non-crypto checks).
