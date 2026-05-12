# April 2026

## Project 1: Execution Witness

### M1 — Finalize ExecutionWitness in execution specs

- [Ignacio] [zkevm@v0.3.4](https://github.com/ethereum/execution-spec-tests/releases/tag/zkevm%40v0.3.4zkevm@v0.3.4) released with multiple fixes and improvements.
- [Ignacio] On-going work: include `public_keys`, proper `chain_config`, and prefix schema-id for proper SSZ-deserialization.


### M2 — Client implementations

#### Besu


#### Ethrex

- [Gaston] [Target zkevm@v0.3.3 fixtures and extend run for all tests](https://github.com/lambdaclass/ethrex/pull/6527) (not only EIP-8025 ones). 

#### Geth


#### Nethermind

- [Team] Making general progress on execution witness generation ([blog post](https://www.nethermind.io/blog/road-to-zk-implementation-nethermind-clients-path-to-proofs))

#### Reth

- [Ignacio] [Support generating legacy and canonical execution witnesses](https://github.com/paradigmxyz/reth/pull/22289) (long due PR merged).
- [lazovicff] [Ongoing work](https://github.com/paradigmxyz/stateless/pull/25) supporting official execution witness releases.

#### Nimbus

 - [deme] Working in running EEST releases.

#### Zilkworm



## Project 2: zkEVM Guest Program

### M1 — Finalize guest program in execution specs

- [Ignacio] [zkevm@v0.3.4](https://github.com/ethereum/execution-spec-tests/releases/tag/zkevm%40v0.3.4) released with multiple fixes and improvements.
- [Ignacio] On-going work to wrap first stable version, in particular: `ChainConfig` and prefix-schema identifier in the stateless input for fork and forward compatibility.
- [Ignacio] Proposed EIP-7709 in ACDE ([presentation](https://docs.google.com/presentation/d/1raEGijf92cUBeCGeTzQusItvIVUAjRcLzzlXtgL9jcU/edit?usp=sharing) & [long-form analysis document](https://hackmd.io/@jsign/EIP-7709-Hegota-proposal)).


### M2 — Reproducible ELF builds and distribution

### M3 — Client implementations
- [Uche] Made a PR implementing a spec compliant guest program using zilkworm (https://github.com/eth-act/ere-guests/pull/38), this was tested using SP1 hypercube in this test [setup](https://github.com/developeruche/z6m-sp1).

#### Besu

- [Gabriel] Found a spec bug regarding incoherent SSZ inputs for tests with invalid txs. This was [fixed](https://github.com/ethereum/execution-specs/pull/2768) in zkevm@v0.3.4.
- [Gabriel] Made zesu support [zkevm@v0.3.4 release](https://github.com/Consensys/zesu/pull/28). 

#### Ethrex

- [Ignacio] Support canonical SSZ bytes as guest program input ([in review](https://github.com/lambdaclass/ethrex/pull/6550)).
- [Ben] [Wire missing zkVM precompile for `hash_tree_root` usage for public inputs](https://github.com/lambdaclass/ethrex/pull/6549)
- [Ben] [Remove rayon](https://github.com/lambdaclass/ethrex/pull/6560) as guest program dependency
- [Ben] [Remove sync](https://github.com/lambdaclass/ethrex/pull/6588) from the guest program execution

#### Geth

#### Nethermind

- [Team] Making general progress on guest program ([blog post](https://www.nethermind.io/blog/road-to-zk-implementation-nethermind-clients-path-to-proofs))
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

- [Francesco] [Reactivated EIP-8025 from Stagnant with a full rewrite](https://github.com/ethereum/EIPs/pull/11604).
- [Francesco] [General refactor of the consensus specs merged](https://github.com/ethereum/consensus-specs/pull/5055): proof node API update, dynamic proof type capability advertisement, and related changes.
- [Francesco] [Bumped `MAX_PROOF_SIZE` to 400 KiB](https://github.com/ethereum/consensus-specs/pull/5162).
- [Francesco] Removed validator proof resigning proposal from the spec — design decision captured in [HackMD writeup](https://hackmd.io/@frisitano/HkCzVt-a-x).
- [Francesco] [Support optional execution and proof engines being considered via optimistic sync](https://github.com/ethereum/consensus-specs/pull/5161).
- [Francesco] [Updated PR for EIP-8025 beacon-APIs endpoints](https://github.com/ethereum/beacon-APIs/pull/569).
- [Francesco] [Proposed opt-in witness retrieval via flag on existing engine methods](https://github.com/ethereum/execution-apis) (execution-apis proposal).
- [Francesco] Opened [`eth-act/execution-proofs-api#1`](https://github.com/eth-act/execution-proofs-api/issues/1) capturing the proof-type encoding discussion.
- [Francesco] Network proof gossip writeup for EIP-8025 — [HackMD](https://hackmd.io/@frisitano/H1XJS3XTZx).

### M2 -- Spec out BiB changes

### M3 -- Client implementations

#### Kurtosis / ethereum-package

- [Francesco] [GPU ere prover support landed in ethereum-package](https://github.com/ethpandaops/ethereum-package/pull/1353), with kurtosis GPU driver / config specification and `shm-size` + ulimits handling.
- [Francesco / Manu] Lighthouse ↔ Prysm interop devnet ran successfully.


#### Lighthouse

- [Francesco] Maintainer-facing Lighthouse architecture writeup for the EIP-8025 implementation — [HackMD](https://hackmd.io/F4RtMrHgSm2Flw8iUbq2xA?view).
- [Francesco] Validator resigning removed from the Lighthouse implementation to align with the spec change.

#### Prysm

#### Grandine

#### Lodestar

#### Nimbus

#### Teku

## Project 5: Prover Infrastructure

### M1 — Integrate zkVMs into Ethproofs

### M2 — Ensure GPU implementations are open source

### M3 — Integrate zkVMs into Ere

- [Han] [Split prover and verifier](https://github.com/eth-act/ere/pull/332) and [release program verification key in `ere-guests`](https://github.com/eth-act/ere-guests/pull/37): The verifier can verify proofs given only the program verification key.
- [Han] [Add crate `ere-verifier` for zkboost in-process verifier integration](https://github.com/eth-act/ere/pull/359)

### M4 — Test zkboost in isolation with single and then multiple GPUs

### M5 — Metrics to track prover reliability and pipelining inefficiencies

### M6 — Allow attesters to use this infrastructure to verify proofs

- [Han] [Auto resoluation of `ere` and `ere-guests` versions for `zkboost` in `ethereum-package`](https://github.com/ethpandaops/ethereum-package/pull/1368)
- [Stefan / Han] [Add in-process ZisK verifier in `zkboost` to verify proofs without spinning up an `ere-server`](https://github.com/eth-act/zkboost/pull/61) and [extend to all zkVM verifiers](https://github.com/eth-act/zkboost/pull/63)

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
