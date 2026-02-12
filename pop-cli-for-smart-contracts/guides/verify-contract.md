---
description: Verify a smart contract by comparing a local build to a reference bundle or an on-chain deployment.
---

# Verify a contract

Use `pop verify` to compare a locally built contract against either a local `.contract` bundle or a deployed contract on a chain.

You can also run the command as `pop v`.
Use this command in a contract project with a `Cargo.toml`. It requires the contract feature in Pop CLI (not available in chain-only builds), plus network access when verifying against a deployed contract.

## Usage

```bash
pop verify [--path <PATH> | PATH] --contract-path <FILE>
pop verify [--path <PATH> | PATH] --url <URL> --address <ADDRESS> --image <IMAGE>
```

## Examples

```bash
# Verify against a local .contract bundle
pop verify --contract-path ./target/ink/my_contract.contract

# Verify against a deployed contract
pop verify \
  --url ws://127.0.0.1:9944 \
  --address 5F3sa2TJ... \
  --image <IMAGE>
```

## Verification modes

`pop verify` requires one of these modes:

- Local reference: `--contract-path <FILE>`
- Deployed reference: `--url <URL> --address <ADDRESS> --image <IMAGE>`

If you omit `--contract-path`, Pop requires all three deployed flags.

## Project path resolution

Use `--path` or the positional `PATH` to point at your contract project. If you omit both, Pop uses the current directory.

## Local reference verification

When you verify against a local `.contract` bundle, Pop reads the bundle metadata to determine how it was built:

- If the bundle includes verifiable build info, Pop requires Docker to rebuild the contract.
- Otherwise, Pop checks that your local toolchain and `cargo contract` version match the bundle, then rebuilds locally.

## Deployed reference verification

When you verify a deployed contract, Pop:

- Connects to the chain at `--url` and fetches the deployed contract code hash.
- Rebuilds your local contract using the `--image` Docker image.
- Compares the rebuilt contract hash to the on-chain hash.

Only chains using the latest revive versions are guaranteed to be supported.

## Output

Pop prints a success message when verification passes:

- Local: `The contract is successfully verified ✅`
- Deployed: `The contract deployed on <URL> at address <ADDRESS> is successfully verified ✅`

## Errors and constraints

- If your workspace contains multiple contracts, verification fails. Run the command against a single contract project.
- Deployed verification fails if the chain does not support revive or the contract address is not found.
- Toolchain and `cargo contract` version mismatches will fail verification for non-verifiable bundles.
