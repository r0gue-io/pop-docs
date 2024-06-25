---
description: Spin up your parachain
---

# up

```bash
pop up <COMMAND>
```

You can spawn a local network using [zombienet](https://github.com/paritytech/zombienet-sdk) as follows:

```bash
pop up parachain -f ./tests/networks/pop.toml -p https://github.com/r0gue-io/pop-node
```

> ℹ️ Pop CLI will automatically source the necessary `polkadot` binaries.

Various examples of network configuration files are available [here](https://github.com/r0gue-io/pop-cli/blob/main/tests/networks).

```bash
pop up parachain --help

Launch a local network

Usage: pop up parachain [OPTIONS] --file <FILE>

Options:
  -f, --file <FILE>
          The Zombienet network configuration file to be used
  -r, --relay-chain <RELAY_CHAIN>
          The version of Polkadot to be used for the relay chain, as per the release tag (e.g. "v1.11.0")
  -s, --system-parachain <SYSTEM_PARACHAIN>
          The version of Polkadot to be used for a system parachain, as per the release tag (e.g. "v1.11.0"). Defaults to the relay chain version if not specified
  -p, --parachain <PARACHAIN>
          The url of the git repository of a parachain to be used, with branch/release tag/commit specified as #fragment (e.g. 'https://github.com/org/repository#ref'). A specific binary name can also be optionally specified via query string parameter (e.g. 'https://github.com/org/repository?binaryname#ref'), defaulting to the name of the repository when not specified
  -c, --cmd <cmd>
          The command to run after the network has been launched
```

### Run a command after the network has been spun up

The following will spin up the network locally according the the zombienet file and once the network is up, it will run the command specified in `--cmd`:

```bash
pop up parachain -f ./tests/networks/pop.toml --cmd ./path/to/my/script
```

