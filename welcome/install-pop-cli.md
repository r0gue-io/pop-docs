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

# Install Pop CLI

### 1. Install Rust and Cargo

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 2. Install Pop CLI

> **Note:** If you want to install Pop CLI for ink! v6 support, see [Getting Started with ink! v6](https://app.gitbook.com/s/CzgRNwlfNkPLJDU555lw/guides/migrating-to-inkv6).

```bash
cargo install --force --locked pop-cli
```

### 3. Set up your environment

```bash
pop install
```
