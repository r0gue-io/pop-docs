---
description: Spin up your parachain
---

# up

```bash
pop up <COMMAND>
```

You can spawn a local network using [zombienet](https://github.com/paritytech/zombienet-sdk).

Here is an example of spinning up Pop Network:

pop.toml

```toml
[relaychain]
chain = "rococo-local"

[[relaychain.nodes]]
name = "alice"
validator = true

[[relaychain.nodes]]
name = "bob"
validator = true

[[parachains]]
id = 4385
default_command = "pop-node"

[[parachains.collators]]
name = "pop"
args = ["-lruntime::contracts=debug"]
```

```bash
pop up parachain -f ./pop.toml
```

> Pop CLI already knows where to source Pop Network binaries from so no need to specify the URL
>
> Also worth mentioning that Pop CLI will automatically source the necessary `polkadot` binaries.

If you would like to spin up a custom parachain then you need to edit the network.toml file accordingly and use the `-p` flag:

```
pop up parachain -f ./tests/networks/my-parachain.toml -p https://github.com/my-username/my-parachain
```

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

