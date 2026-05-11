# April 2026

## Project 1: Execution Witness

### M1 — Finalize ExecutionWitness in execution specs

- [Ignacio] [zkevm@v0.3.4](https://github.com/ethereum/execution-spec-tests/releases/tag/zkevm%40v0.3.4zkevm@v0.3.4) released with multiple fixes and improvements.
- [Ignacio] On-going work: include `public_keys`, proper `chain_config`, and prefix schema-id for proper SSZ-deserialization.


### M2 — Client implementations

#### Besu


#### Ethrex

- [Gaston] https://github.com/lambdaclass/ethrex/pull/6527Target zkevm@v0.3.3 fixtures and extend run for all tests (not only EIP-8025 ones). 

#### Geth


#### Nethermind

- [Team] Making general progress on execution witness generation (https://www.nethermind.io/blog/road-to-zk-implementation-nethermind-clients-path-to-proofsblog post)

#### Reth

- [Ignacio] Support generating legacy and canonical execution witnesses (https://github.com/paradigmxyz/reth/pull/22289long due PR merged).
- [lazovicff] [Ongoing work](https://github.com/paradigmxyz/stateless/pull/25) supporting official execution witness releases.

#### Zilkworm


## Project 2: zkEVM Guest Program

### M1 — Finalize guest program in execution specs

- [Ignacio] https://github.com/ethereum/execution-spec-tests/releases/tag/zkevm%40v0.3.4zkevm@v0.3.4 released with multiple fixes and improvements.
- [Ignacio] On-going work to wrap first stable version, in particular: `ChainConfig` and prefix-schema identifier in the stateless input for fork and forward compatibility.
- [Ignacio] Proposed EIP-7709 in ACDE ([presentation](https://docs.google.com/presentation/d/1raEGijf92cUBeCGeTzQusItvIVUAjRcLzzlXtgL9jcU/edit?usp=sharing) & [long-form analysis document](https://hackmd.io/@jsign/EIP-7709-Hegota-proposal)).


### M2 — Reproducible ELF builds and distribution

### M3 — Client implementations

#### Besu

- [Gabriel] Found a spec bug regarding incoherent SSZ inputs for tests with invalid txs. This was [fixed](https://github.com/ethereum/execution-specs/pull/2768) in zkevm@v0.3.4.
- [Gabriel] Made zesu support [zkevm@v0.3.4 release](https://github.com/Consensys/zesu/pull/28). 

#### Ethrex

- [Ignacio] Support canonical SSZ bytes as guest program input ([in review](https://github.com/lambdaclass/ethrex/pull/6550)).
- [Ben] [Wire missing zkVM precompile for `hash_tree_root` usage for public inputs](https://github.com/lambdaclass/ethrex/pull/6549)
- [Ben] [Remove rayon](https://github.com/lambdaclass/ethrex/pull/6560) as guest program dependency

#### Geth

#### Nethermind

- [Team] Making general progress on guest program (https://www.nethermind.io/blog/road-to-zk-implementation-nethermind-clients-path-to-proofsblog post)
- [Ruben] Helping discuss upcoming guest program input design.

#### Reth

#### Zilkworm

- [Som] Experimenting with alternative guest input design that can become more performant.


## Project 3: zkEVM Guest API standardization

### M1 — Standardize minimal hardware targets


### M2 — Standardize zkVM precompiles via C headers

### M3 — Standardize the interface for accessing IO via C headers

### M4 — Standardize assumptions about the ELF and zkVM processing

## Project 4: Consensus Layer Integration

### M1 — Finalize consensus specs

### M2 -- Spec out BiB changes

### M3 -- Client implementations

#### Kurtosis / ethereum-package

#### Lighthouse

#### Prysm

#### Grandine

#### Lodestar

#### Nimbus

#### Teku

## Project 5: Prover Infrastructure

### M1 — Integrate zkVMs into Ethproofs

### M2 — Ensure GPU implementations are open source

### M3 — Integrate zkVMs into Ere

### M4 — Test zkboost in isolation with single and then multiple GPUs

### M5 — Metrics to track prover reliability and pipelining inefficiencies

### M6 — Allow attesters to use this infrastructure to verify proofs

## Project 6: Benchmarking and Metrics

### M1 — Benchmark guest programs against zkVMs locally

- [Ignacio] New Zisk v0.16.1 run for refreshed EEST benchmarks and mainnet blocks ([dashboards landing page](https://eth-act.github.io/zkevm-benchmark-runs/)).
- [Ignacio] [New Zisk profiling dashboard](https://eth-act.github.io/zkevm-benchmark-runs/profiles/) for easier analysis of any EEST benchmark profiling.
- [Ignacio] Improved [_Repricing dashboard_](https://eth-act.github.io/zkevm-benchmark-runs/repricing/): crash message details, unified dataset ux, plot view with proving time per gas cost (linearity), and other improvements.
- [Ignacio] [Found bug and implemented fix](https://github.com/CPerezz/worst_case_miner/pull/7) in worst_case_miner used in EEST for deep SSTORE benchmarks.
- [Ignacio] [Implemented new SLOAD deep branch benchmark](https://github.com/ethereum/execution-specs/pull/2635) in EEST.

### M2 — Integrate metrics into pandaOps' lab

### M3 — Investigate multidimensional metering/pricing

- [Ignacio / Maria] Brainstormed potential improvements to Glamsterdam repricing analysis tool to generalize it for proving and H* upcoming projects.

## Project 7: Security

### M1 — Agree on proof size, security regime and timelines

### M2 — Figure out what components need specifications

### M3 — Figure out what the end-game for specs looks like

### M4 — Figure out guarantees around each component

### M5 — Establish a minimal go/no-go framework

### M6 — Identify minimum acceptable formal verification requirements

### M7 — Establish security and adversary model

### M8 — Establish and test safe fallback behavior
