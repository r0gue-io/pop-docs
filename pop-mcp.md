---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# Pop MCP

Pop MCP is an MCP server that exposes Pop CLI as structured tools for AI clients (contract + chain workflows).

## Prerequisites

- Pop CLI installed and working:

```bash
pop --version
```

If needed: [Install Pop CLI](welcome/install-pop-cli.md)

## Install

### Option A: Build from source

```bash
git clone https://github.com/r0gue-io/pop-mcp.git
cd pop-mcp
cargo build --release
```

Server binary:
`target/release/pop-mcp-server`

### Option B: Download a prebuilt binary

Get a release for your platform from GitHub Releases:
`https://github.com/r0gue-io/pop-mcp/releases`

## Run The Server

Pop MCP runs over stdio. In practice, your MCP client launches the server process for you.

If you run it directly, it will wait for MCP messages on stdin:

```bash
pop-mcp-server
```

## Connect A Client

Pop MCP uses `PRIVATE_KEY` for signing (only needed when you set `execute=true` on write calls).

Use dev keys like `//Alice` and `//Bob` for local networks only.

### Claude Code (CLI)

Project scope (writes `.mcp.json` in the repo):

```bash
claude mcp add pop-mcp --scope project --env PRIVATE_KEY=//Alice -- /absolute/path/to/pop-mcp/target/release/pop-mcp-server
```

Verify:

```bash
claude mcp get pop-mcp
```

### Codex CLI

Config is stored in `~/.codex/config.toml` (or project `.codex/config.toml`).

```bash
codex mcp add pop-mcp --env PRIVATE_KEY=//Alice -- /absolute/path/to/pop-mcp/target/release/pop-mcp-server
```

Verify:

```bash
codex mcp list
```

## Try It (Copy/Paste Workflows)

### 1) Sanity check Pop CLI access

In your MCP client, call:

```text
check_pop_installation
```

### 2) Local ink! node + deploy a flipper contract + read state

```text
list_templates
create_contract {"name":"mcp_flipper","template":"standard"}
build_contract {"path":"./mcp_flipper","release":true}
up_ink_node {}
deploy_contract {"path":"./mcp_flipper"}

# Copy the deployed contract address from deploy output, then:
call_contract {"path":"./mcp_flipper","contract":"<CONTRACT_ADDRESS>","message":"get"}
```

To flip state (requires `PRIVATE_KEY` configured and `execute=true`):

```text
call_contract {"path":"./mcp_flipper","contract":"<CONTRACT_ADDRESS>","message":"flip","execute":true}
```

### 3) Local Paseo + Asset Hub transfer + events

```text
up_network {"chain":"paseo","parachain":["asset-hub#1000:9944"]}
call_chain {"url":"ws://localhost:9944","metadata":true}
call_chain {"url":"ws://localhost:9944","pallet":"Balances","function":"ExistentialDeposit"}

# Transfer (requires `PRIVATE_KEY` configured and execute=true)
call_chain {"url":"ws://localhost:9944","pallet":"Balances","function":"transferKeepAlive","args":["<BOB_SS58>","1000000000000"],"execute":true}
call_chain {"url":"ws://localhost:9944","pallet":"System","function":"Events"}
```

Clean up:

```text
clean_nodes {"pids":[<PID1>,<PID2>]}
clean_network {"path":"<PATH_TO_ZOMBIE_JSON_OR_BASE_DIR>"}
```

## References

- Pop MCP repo: `https://github.com/r0gue-io/pop-mcp`
- MCP spec: `https://modelcontextprotocol.io`

