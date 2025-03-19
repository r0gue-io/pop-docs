---
description: Benchmark a chain
---

# bench

```bash
pop bench <COMMAND>
```

There are several commands available for benchmarking a chain:

- `block`: Benchmark the execution time of historic blocks.
- `machine`: Benchmark the machine performance.
- `overhead`: Benchmark the execution overhead per-block and per-extrinsic.
- `pallet`: Benchmark the extrinsic weight of pallets.
- `storage`: Benchmark the storage speed of a chain snapshot.

## bench block

To benchmark the execution time of historic blocks

```bash
pop bench block --from <FROM> --to <TO>
```

The command requires arguments `FROM` and `TO` where `FROM` is the number of the first block and `TO` is the last block number.

***Note***: Parachain node needs to be built with `runtime-benchmarks` feature enabled.

> Pop CLI will automatically locate the node binary based on the provided `--profile`. If no binary found, Pop CLI will automatically build the parachain node.

## bench machine

To benchmark the machine performance:

```bash
pop bench machine
```

***Note***: Parachain node needs to be built with `runtime-benchmarks` feature enabled.

> Pop CLI will automatically locate the runtime binary based on the provided `--profile`. If no binary found, Pop CLI will automatically build the parachain runtime.

## bench overhead

To benchmark the execution overhead per-block and per-extrinsic:

```bash
pop bench overhead
```

***Note***: Parachain runtime needs to be built with `runtime-benchmarks` feature enabled.

> Pop CLI will automatically locate the node binary based on the provided `--profile`. If no binary found, Pop CLI will automatically build the parachain node.

The command requires the `frame-omni-bencher` binary to be installed on your local machine.

> Pop CLI will automatically source the `frame-omni-bencher` binary if no binary found on your local machine.

## bench pallet
**Benchmark pallets and extrinsics**

```bash
pop bench pallet
```


To benchmark pallets and extrinsics of the runtime, you will be prompted to provide a valid runtime's binary path, make sure the binary is built with `--runtime-benchmarks` feature.

{% hint style="info" %}
Pop CLI only supports benchmarking pallets and extrinsics with runtime binary. There is no option to benchmark with chain specs. This feature is similar to the approach of [`frame-omni-bencher`](https://crates.io/crates/frame-omni-bencher) with interactive interface.
{% endhint %}

After the binary path is located, you will be prompted to select a pallet, the dispatchable functions and the supported arguments. All arguments can be managed via parameter menu:

```json
◆  Select the parameter to update:
│  ● (0) - Pallets: pallet_timestamp
│  ○ (1) - Extrinsics: All selected
│  ○ (2) - Runtime path: ./target/release/runtime.wasm
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
│  ○ (15) - Weight file template:
│  ○ > Save all parameter changes and continue
```


If interactive guidance is not desired, you can proceed manually as follows:

```bash
pop bench pallet --pallet=pallet_timestamp --extrinsic= --steps=50 --runtime=./target/release/runtime.wasm --genesis-builder=runtime --output=./weights.rs
```

If you want to skip the parameter menu, use the `--skip-menu` flag:


```bash
pop bench pallet --pallet=pallet_timestamp --extrinsic= --steps=50 --runtime=./target/release/runtime.wasm --genesis-builder=runtime --output=./weights.rs --skip-menu
```

Parameters can be reused later by saving it to the `pop-bench.toml`, learn more [here](../guides/benchmarking-a-parachain/benchmarking-pallets-and-extrinsics.md). To reuse the saved parameters, use the `-f` or `--bench-file` flag:

```bash
pop bench pallet -f pop-bench.toml
```

or

```bash
pop bench pallet --bench-file pop-bench.toml
```

To save the generated weights, you need to provide path to the weight file to `--output`:

```bash
pop bench pallet --pallet=pallet_timestamp --extrinsic= --steps=50 --runtime=./target/release/runtime.wasm --genesis-builder=runtime --output=./weights.rs
```

**List available pallets and extrinsics**

To list all available pallets and extrinsics for benchmarking, use the `--list` flag:
```bash
pop bench pallet --list
```

### Additional options:

``` bash
pop bench pallet --help

Benchmark the extrinsic weight of pallets.

Usage: pop bench pallet [OPTIONS]

Options:
  -p, --pallet <PALLET>
          Select a pallet to benchmark, or `*` for all (in which case
          `extrinsic` must be `*`)

  -e, --extrinsic <EXTRINSIC>
          Select an extrinsic inside the pallet to benchmark, or `*` for all

      --exclude-pallets <EXCLUDE_PALLETS>...
          Comma separated list of pallets that should be excluded from the
          benchmark

      --all
          Run benchmarks for all pallets and extrinsics.

          This is equivalent to running `--pallet * --extrinsic *`.

  -s, --steps <STEPS>
          Select how many samples we should take across the variable components

          [default: 50]

      --low <LOWEST_RANGE_VALUES>
          Indicates lowest values for each of the component ranges

      --high <HIGHEST_RANGE_VALUES>
          Indicates highest values for each of the component ranges

  -r, --repeat <REPEAT>
          Select how many repetitions of this benchmark should run from within
          the wasm

          [default: 20]

      --external-repeat <EXTERNAL_REPEAT>
          Select how many repetitions of this benchmark should run from the
          client.

          NOTE: Using this alone may give slower results, but will afford you
          maximum Wasm memory.

          [default: 1]

      --json
          Print the raw results in JSON format

      --json-file <JSON_FILE>
          Write the raw results in JSON format into the given file

      --no-median-slopes
          Don't print the median-slopes linear regression analysis

      --no-min-squares
          Don't print the min-squares linear regression analysis

      --output <OUTPUT>
          Output the benchmarks to a Rust file at the given path

      --template <TEMPLATE>
          Path to Handlebars template file used for outputting benchmark
          results. (Optional)

      --output-analysis <OUTPUT_ANALYSIS>
          Which analysis function to use when outputting benchmarks: *
          min-squares (default) * median-slopes * max (max of min squares and
          median slopes for each value)

      --output-pov-analysis <OUTPUT_POV_ANALYSIS>
          Which analysis function to use when analyzing measured proof sizes

          [default: median-slopes]

      --heap-pages <HEAP_PAGES>
          Set the heap pages while running benchmarks. If not set, the default
          value from the client is used

      --no-verify
          Disable verification logic when running benchmarks

      --extra
          Display and run extra benchmarks that would otherwise not be needed
          for weight construction

      --runtime <RUNTIME>
          Optional runtime blob to use instead of the one from the genesis
          config

      --allow-missing-host-functions
          Do not fail if there are unknown but also unused host functions in the
          runtime

      --genesis-builder <GENESIS_BUILDER>
          How to construct the genesis state

          Possible values:
          - none:    Do not provide any genesis state
          - runtime: Let the runtime build the genesis state through its
            `BuildGenesisConfig` runtime API

      --genesis-builder-preset <GENESIS_BUILDER_PRESET>
          The preset that we expect to find in the GenesisBuilder runtime API.

          This can be useful when a runtime has a dedicated benchmarking preset
          instead of using the default one.

          [default: development]

      --db-cache <MiB>
          Limit the memory the database cache can use

          [default: 1024]

      --list
          List and print available benchmarks in a csv-friendly format

      --no-storage-info
          If enabled, the storage info is not displayed in the output next to
          the analysis.

          This is independent of the storage info appearing in the *output
          file*. Use a Handlebar template for that purpose.

      --map-size <WORST_CASE_MAP_VALUES>
          The assumed default maximum size of any `StorageMap`.

          When the maximum size of a map is not defined by the runtime
          developer, this value is used as a worst case scenario. It will affect
          the calculated worst case PoV size for accessing a value in a map,
          since the PoV will need to include the trie nodes down to the
          underlying value.

          [default: 1000000]

      --additional-trie-layers <ADDITIONAL_TRIE_LAYERS>
          Adjust the PoV estimation by adding additional trie layers to it.

          This should be set to `log16(n)` where `n` is the number of top-level
          storage items in the runtime, eg. `StorageMap`s and `StorageValue`s. A
          value of 2 to 3 is usually sufficient. Each layer will result in an
          additional 495 bytes PoV per distinct top-level access. Therefore
          multiple `StorageMap` accesses only suffer from this increase once.
          The exact number of storage items depends on the runtime and the
          deployed pallets.

          [default: 2]

      --disable-proof-recording
          Do not enable proof recording during time benchmarking.

          By default, proof recording is enabled during benchmark execution.
          This can slightly inflate the resulting time weights. For parachains
          using PoV-reclaim, this is typically the correct setting. Chains that
          ignore the proof size dimension of weight (e.g. relay chain,
          solo-chains) can disable proof recording to get more accurate results.

      --skip
          If this is set to true, no parameter menu pops up

  -y, --skip-confirm
          Automatically source the needed binary required without prompting for
          confirmation

  -f, --bench-file <BENCH_FILE>
          Output file of the benchmark parameters

  -h, --help
          Print help (see a summary with '-h')
```

## bench storage

To benchmark the storage speed of a chain snapshot:

```bash
pop bench storage --state-version <STATE_VERSION>
```

The command requires a state version to be specified. Substrate `--dev` should use `V1` and Polkadot `V0`. Selecting the wrong version can corrupt the DB. For example, to specify the state version for a Substrate `--dev` chain:

```bash
pop bench storage --state-version 1
```

***Note***: Parachain node needs to be built with `runtime-benchmarks` feature enabled.

> Pop CLI will automatically locate the node binary based on the provided `--profile`. If no binary found, Pop CLI will automatically build the parachain node.
