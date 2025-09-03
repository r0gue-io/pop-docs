---
description: Convert addresses between Ethereum and Polkadot formats seamlessly when building cross-chain applications with Pop CLI.
---

# Address Conversion with Pop CLI

When building applications that bridge Ethereum and Polkadot ecosystems, you'll frequently need to convert addresses
between formats. The `pop convert address` command provides a reliable way to transform Ethereum addresses to
Substrate/Polkadot addresses and vice versa.

## What you can use it for

- Convert Ethereum addresses to Polkadot/Substrate format for cross-chain operations.
- Transform Polkadot addresses back to Ethereum format for EVM compatibility layers.
- Generate addresses with different SS58 prefixes for various Substrate-based chains.
- Validate address formats and ensure proper cross-chain address mapping.
- Facilitate user experience in multi-chain applications by supporting both address formats.

## Quick examples

```bash
# Convert an Ethereum address to Polkadot format (default prefix 0)
pop convert address 0x742d35Cc6634C0532925a3b844Bc454e4438f44e

# Convert to a specific Substrate chain format (e.g., Kusama prefix 2)
pop convert address 0x742d35Cc6634C0532925a3b844Bc454e4438f44e 2

# Convert a Polkadot address back to Ethereum format
pop convert address 13dKz82CEiU7fKfhfQ5aLpdbXHApLfJH5Z6y2RTZpRwKiNhX

# Convert addresses from different Substrate networks
pop convert address 5Eh2qnm8NwCeDnfBhm2aCfoSffBAeMk914NUs8UDGLuoY6qg
```

## How it works

The conversion uses a standardized mapping between Ethereum and Substrate address formats:

- **Ethereum → Substrate**: Takes the 20-byte Ethereum address and extends it with 12 bytes of `0xEE` padding, then
  encodes it using SS58 format.
- **Substrate → Ethereum**: Decodes the SS58 address, verifies the last 12 bytes are `0xEE` (indicating it originated
  from an Ethereum address), and extracts the first 20 bytes as the Ethereum address.

## Tips

- **SS58 prefixes**: Different Substrate chains use different prefixes (Polkadot: 0, Kusama: 2, Generic Substrate: 42).
  The tool defaults to Polkadot (0) if no prefix is specified.
- **Case sensitivity**: Ethereum addresses are case-insensitive for conversion purposes.
- **Validation**: The tool validates address formats and will reject invalid inputs with clear error messages.
- **Bidirectional**: Pop automatically detects whether you're providing an Ethereum (0x-prefixed) or Substrate address
  and converts accordingly.
- **Cross-chain compatibility**: Only Substrate addresses originally derived from Ethereum addresses can be converted
  back (they must have the `0xEE` padding in the last 12 bytes).
