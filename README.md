# Madara App Chain Template

A fresh [Madara](https://github.com/keep-starknet-strange/madara) app chain, ready for hacking 🚀

All bugs, suggestions, and feature requests should be made upstream in the [Madara](https://github.com/keep-starknet-strange/madara) repository.

## Getting Started

Depending on your operating system and Rust version, there might be additional packages required to compile this template.
Check the [Install](https://docs.substrate.io/install/) instructions for your platform for the most common dependencies.
Alternatively, you can use one of the [alternative installation](#alternatives-installations) options.

### Build

Use the following command to build the node without launching it:

```sh
cargo build --release
```

### Embedded Docs

After you build the project, you can use the following command to explore its parameters and subcommands:

```sh
./target/release/app-chain-node -h
```

You can generate and view the [Rust Docs](https://doc.rust-lang.org/cargo/commands/cargo-doc.html) for this template with this command:

```sh
cargo +nightly doc --open
```

### Single-Node Development Chain

Set up the chain with the genesis config. More about defining the genesis state is mentioned below.

```sh
./target/release/app-chain-node setup --chain dev --from-local ./configs
```

The following command starts a single-node development chain.

```sh
./target/release/app-chain-node --dev
```

You can specify the folder where you want to store the genesis state as follows

```sh
./target/release/app-chain-node setup --chain dev --from-local ./configs --base-path=<path>
```

If you used a custom folder to store the genesis state, you need to specify it when running

```sh
./target/release/app-chain-node --base-path=<path>
```

Please note, Madara overrides the default `dev` flag in substrate to meet its requirements. The following flags are automatically enabled with the `--dev` argument:

`--chain=dev`, `--force-authoring`, `--alice`, `--tmp`, `--rpc-external`, `--rpc-methods=unsafe`

To store the chain state in the same folder as the genesis state, run the following command. You cannot combine the `base-path` command
with `--dev` as `--dev` enforces `--tmp` which will store the db at a temporary folder. You can, however, manually specify all flags that
the dev flag adds automatically. Keep in mind, the path must be the same as the one you used in the setup command.

```sh
./target/release/app-chain-node --base-path <path>
```

To start the development chain with detailed logging, run the following command:

```sh
RUST_BACKTRACE=1 ./target/release/app-chain-node -ldebug --dev
```

### Connect with Polkadot-JS Apps Front-End

After you start the app chain locally, you can interact with it using the hosted version of the [Polkadot/Substrate Portal](https://polkadot.js.org/apps/#/explorer?rpc=ws://localhost:9944) front-end by connecting to the local node endpoint.
A hosted version is also available on [IPFS (redirect) here](https://dotapps.io/) or [IPNS (direct) here](ipns://dotapps.io/?rpc=ws%3A%2F%2F127.0.0.1%3A9944#/explorer).
You can also find the source code and instructions for hosting your own instance on the [polkadot-js/apps](https://github.com/polkadot-js/apps) repository.

### Multi-Node Local Testnet

If you want to see the multi-node consensus algorithm in action, see [Simulate a network](https://docs.substrate.io/tutorials/get-started/simulate-network/).

## Template Structure

The app chain template gives you complete flexibility to modify exiting features of Madara and add new features as well.

### Existing Pallets

Madara comes with only one pallet - `pallet_starknet`. This pallet allows app chains to execute Cairo contracts and have 100% RPC compatabiltiy with Starknet mainnet. This means all Cairo tooling should work out of the box with the app chain. At the same time, the pallet also allows the app chain to fine tune specific parameters to meet their own needs.

- `DisableTransactionFee`: If true, calculate and store the Starknet state commitments
- `DisableNonceValidation`: If true, check and increment nonce after a transaction
- `InvokeTxMaxNSteps`: Maximum number of Cairo steps for an invoke transaction
- `ValidateMaxNSteps`: Maximum number of Cairo steps when validating a transaction
- `MaxRecursionDepth`: Maximum recursion depth for transactions
- `ChainId`: The chain id of the app chain

All these options can be configured inside `crates/runtime/src/pallets.rs`

### Genesis

The genesis state of the app chain is defined via a JSON file. It lives inside the `config` folder but can also be fetch from a url. You can read in more detail about how to setup the config from these two docs in the Madara repo.
- [Genesis](https://github.com/keep-starknet-strange/madara/blob/main/docs/genesis.md)
- [Configs](https://github.com/d-roak/madara/blob/feat/configs-index/docs/configs.md)

### New Pallets

Adding a new pallet is the same as adding a pallet in any substrate based chain. An an example, `pallet-template` has been added on this template. You can read more about it [here](https://docs.substrate.io/tutorials/build-application-logic/add-a-pallet/).

### Runtime configuration

Similar to new pallets, runtime configurations can be just like they're done in Substrate. You can edit all the available parameters inside `crates/runtime/src/config.rs`.

For example, to change the block time, you can edit the `MILLISECS_PER_BLOCK` variable.

## Alternatives Installations

Instead of installing dependencies and building this source directly, consider the following alternatives.

### Nix

Install [nix](https://nixos.org/), and optionally [direnv](https://github.com/direnv/direnv) and [lorri](https://github.com/nix-community/lorri) for a fully plug-and-play experience for setting up the development environment.
To get all the correct dependencies, activate direnv `direnv allow` and lorri `lorri shell`.

### Docker

Please use the [Madara Dockerfile](https://github.com/keep-starknet-strange/madara/blob/main/Dockerfile) as a reference to build the Docker container with your App Chain node as a binary.
