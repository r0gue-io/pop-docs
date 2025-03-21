# Benchmarking Pallets and Extrinsics

Benchmarking in Substrate measures execution time and resource usage for pallets and extrinsics, ensuring accurate weight calculations and optimal performance.

With Pop CLI, you can benchmark pallets and extrinsics interactively by managing parameters efficiently. Run the following command to start benchmarking:

``` bash
pop bench pallet
```

Note that the command requires the `frame-omni-bencher` binary to be installed on your local machine.

> Pop CLI will automatically source the `frame-omni-bencher` binary if no binary found on your local machine.

**Provide a runtime to benchmark**

The command requires a runtime built with the `runtime-benchmarks` feature. Pop CLI detects available runtimes in your project, allowing you to choose one if multiple exist.

```bash
◇  Choose the build profile of the binary that should be used:
│  Release
│
◆  Select the runtime:
│  ○ pop-runtime-testnet
│  ● pop-runtime-devnet
│  ○ pop-runtime-mainnet
└
```

If the binary is missing, Pop CLI builds it with the appropriate `profile` and features. You can manually specify the runtime binary path using:

```bash
pop bench pallet --runtime=target/release/pop-runtime-devnet.wasm
```

**Configure the genesis builder policy and preset**

```bash
◆  Select the genesis builder policy:
│  ● none (Do not provide any genesis state.)
│  ○ runtime
└
```

To configure the genesis builder manually, you can use the `--genesis-builder` flag. For example:

```bash
pop bench pallet --runtime=target/release/pop-runtime-devnet.wasm --genesis-builder=runtime
```

If the policy is `runtime`, means that Pop CLI will benchmark your runtime with a genesis preset on the runtime.

```bash
◇  Select the genesis builder policy:
│  runtime
│
◇  Found 2 genesis builder presets
│
◆  Select the genesis builder preset:
│  ● development
│  ○ local_testnet
└
```

To configure the genesis builder preset, you can use the `--genesis-builder-preset` flag:

```bash
pop bench pallet --runtime=target/release/pop-runtime-devnet.wasm --genesis-builder=runtime --genesis-builder-preset=development
```

**Select pallets and extrinsics**

You'll be prompted to benchmark all pallets or select a specific one. Pop CLI lists available pallets, allowing you to search by name and choose from the list.

```bash
◆  🔎 Search for a pallet to benchmark
│
│  ● cumulus_pallet_parachain_system
│  ○ cumulus_pallet_xcmp_queue
│  ○ frame_system
│  ○ pallet_balances
│  ○ pallet_collator_selection
│  ○ pallet_message_queue
│  ○ pallet_session
│  ○ pallet_sudo
│  ○ pallet_timestamp
└
```

You can simply search for pallets by typing their name and selecting them from the list.
```bash
◆  🔎 Search for a pallet to benchmark
│  pallet_time
│  ● pallet_timestamp
│  ○ pallet_session
│  ○ pallet_sudo
│  ○ pallet_message_queue
│  ○ pallet_balances
│  ○ pallet_collator_selection
└
```

If a pallet is specified, the CLI will prompt you to select the extrinsics you want to benchmark within that pallet. You can select multiple extrinsics by pressing the spacebar. Same as searching for pallets, you can search for extrinsics by typing their name as well.

```bash
◆  🔎 Search for extrinsics to benchmark (select with space)
│
│  ◼ burn_allow_death
│  ◼ burn_keep_alive
│  ◼ force_adjust_total_issuance
│  ◼ force_set_balance_creating
│  ◻ force_set_balance_killing
│  ◻ force_transfer
│  ◻ force_unreserve
│  ◻ transfer_all
│  ◻ transfer_allow_death
│  ◻ transfer_keep_alive
│  ◻ upgrade_accounts
└
```

If `--extrinsic=` and `--pallet=` are provided, the CLI will skip the search and directly benchmark with the specified arguments.

**Manage parameters using the menu**

Pop CLI displays all editable parameters in a menu, allowing you to preview and modify them as needed. Simply select a parameter to update.

> To skip the parameter menu, you can use the `--skip-menu` flag.

```bash
◆  Select the parameter to update:
│  ○ (0) - Pallets: pallet_balances
│  ○ (1) - Extrinsics: 4 selected
│  ○ (2) - Runtime path: tests/runtimes/base_parachain_benchmark.wasm
│  ○ (3) - Genesis builder: runtime
│  ○ (4) - Genesis builder preset: development
│  ○ (5) - Steps: 50
│  ○ (6) - Repeats: 20
│  ○ (7) - High: None
│  ○ (8) - Low: None
│  ○ (9) - Map size: 1000000
│  ○ (10) - Database cache size: 1024
│  ○ (11) - Additional trie layer: 2
│  ○ (12) - No median slope: false
│  ○ (13) - No min square: false
│  ○ (14) - No storage info: false
│  ○ (15) - Weight file template: None
│  ● > Save all parameter changes and continue
└
```

For example, to update the `High` parameter:

```bash
◇  Select the parameter to update:
│  (7) - High: None
│
◆  Provide range values to the parameter "High" (numbers separated by commas)
│  10,50,100
└
```

And you will see the value is updated in the menu:

```bash
│  ○ (6) - Repeats: 20
│  ● (7) - High: 10,50,100
│  ○ (8) - Low: None
```

**Save weight output and parameters to file**

After the parameter menu, you will be prompted to save the weight output and parameters to a file. If not provided, no weight output will be saved.

```bash
◆  Provide the output file path for benchmark results (optional).
│  ./weight.rs
└
```

To save parameters, the file path is default to `pop-bench.toml`.

```bash
◆  Provide the output path for benchmark parameters
│  pop-bench.toml
└
```

Below is the example of the parameter file:

```toml
version = "1"
pallet = "pallet_timestamp"
extrinsic = "*"
exclude_pallets = []
all = false
steps = 50
lowest_range_values = []
highest_range_values = []
repeat = 20
external_repeat = 1
json_output = false
no_median_slopes = false
no_min_squares = false
output_pov_analysis = "median-slopes"
no_verify = false
extra = false
runtime = "runtimes/base_parachain_benchmark.wasm"
allow_missing_host_functions = false
genesis_builder = "Runtime"
genesis_builder_preset = "development"
database_cache_size = 1024
list = false
no_storage_info = false
worst_case_map_values = 1000000
additional_trie_layers = 2
disable_proof_recording = false
skip_menu = false
skip_confirm = false
```

With the saved parameter file, you can load parameters from the file by using the `-f` or `--bench-file` flag.

```bash
pop bench -f pop-bench.toml
```

For more advanced parameters configuration, you can run the following command to learn more:

```bash
pop bench --help
```

#### Learning Resources

* 🧑‍🏫 To learn about Polkadot in general, [Polkadot.network](https://polkadot.network/) website is a good starting point.
  * ⭕ Learn more about parachains [here](https://wiki.polkadot.network/docs/learn-parachains).
* 🧑‍🔧 For technical introduction, [here](https://github.com/paritytech/polkadot-sdk#-documentation) are the Polkadot SDK documentation resources.

* To learn about benchmarking, [Polkadot Docs - Benchmarking](https://docs.polkadot.com/develop/parachains/testing/benchmarking/) provides all the fundamentals.
* More advanced breakdown of benchmarking is covered in [Polkadot SDK Docs - Frame Benchmarking Weight](https://paritytech.github.io/polkadot-sdk/master/polkadot_sdk_docs/reference_docs/frame_benchmarking_weight/index.html).

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)
