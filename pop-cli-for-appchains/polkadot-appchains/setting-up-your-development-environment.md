---
description: >-
  This section will guide on how to use Pop CLI to set up your development
  environment for Polkadot appchain development.
---

# Setting up your development environment

In order to develop and build a Polkadot appchain we need to setup the proper environment.

Luckily we can use Pop CLI for this. Pop CLI has the `pop install` command.

```
pop install --help

Set up the environment for development by installing required packages

Usage: pop install [OPTIONS]

Options:
  -y, --skip-confirm  Automatically install all dependencies required without prompting for confirmation
  -h, --help          Print help
```

To install all the packages on your system that are needed for Polkadot appchain development, run the following command:

```bash
pop install
```

```
┌   Pop CLI : Install dependencies for development
│
⚙  ℹ️ Mac OS (Darwin) detected.
│  
⚙  More information about the packages to be installed here: https://docs.substrate.io/install/macos/
│  
◆  📦 Do you want to proceed with the installation of the following packages: homebrew, protobuf, openssl, rustup and cmake ?
│  ● Yes  / ○ No 
└

```

The above command will install all the necessary packages.



If you run into any errors, please report them as an issue in the Pop CLI repo so we can better improve the tool:

* [https://github.com/r0gue-io/pop-cli/issues](https://github.com/r0gue-io/pop-cli/issues)
