---
description: Convert addresses between Ethereum and Substrate formats using Pop CLI.
---

# Address conversion

Use `pop convert address` to convert between Ethereum addresses and Substrate (SS58) addresses.

You can also run the command as `pop cv address` or `pop convert a`.

## Usage

```bash
pop convert address <ADDRESS>
```

## Examples

```bash
# Convert an Ethereum address to Substrate (SS58 prefix 0)
pop convert address 0x742d35Cc6634C0532925a3b844Bc454e4438f44e

# Convert a Substrate address back to Ethereum
pop convert address 13dKz82CEiU7fKfhfQ5aLpdbXHApLfJH5Z6y2RTZpRwKiNhX

# Convert a 32-byte public key (0x-prefixed) to Ethereum
pop convert address 0x8eaf04151687736326c9fea17e25fc5287613693c912909cb226aa4794f26a48
```

## Input formats

`<ADDRESS>` can be one of:

- Ethereum address: `0x` + 40 hex characters.
- Public key: `0x` + 64 hex characters.
- Substrate address: SS58-encoded string.

## How conversion works

- **Ethereum → Substrate**: Pop appends 12 bytes of `0xEE` to the 20-byte Ethereum address and encodes the result as SS58 with prefix `0` (Polkadot).
- **Substrate/Public key → Ethereum**:
  - If the last 12 bytes are `0xEE`, Pop treats it as an Ethereum-derived address and returns the first 20 bytes.
  - Otherwise, Pop computes `keccak256` of the 32-byte public key and returns the last 20 bytes.

## Output

Pop prints the converted address to stdout. Ethereum outputs are `0x`-prefixed lowercase hex.

## Errors and constraints

- Ethereum inputs must be `0x`-prefixed and exactly 40 hex characters.
- Public keys must be `0x`-prefixed and exactly 64 hex characters.
- Invalid SS58 strings fail validation.
