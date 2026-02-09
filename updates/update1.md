# Update 1

> Please also link updates to the milestones in the planning doc

## Project 1: Execution Witness

### M1 — Finalize ExecutionWitness in execution specs

- [Peter] Created a [draft state tracker PR](https://github.com/ethereum/execution-specs/pull/2063). This will be required for execution witness generation.
- [Ignacio] [Built on top for also tracking bytecodes and ancestor headers](https://github.com/petertdavies/execution-specs/pull/5), since this is also required for execution witness generation.
- [Ignacio] Implemented an [e2e execution witness generation](https://github.com/jsign/execution-specs/pull/1) in the spec. It has its own state tracker that we could replace with Peter's state tracker, but also has many new components required for execution witness generation (e.g. MPT pre-state and branch colapse node detection, integration with testing framework, etc). Main goal is to help discussing how to move forward in steps in the spec.
- [Ignacio] Wrote a deep dive into [OpenVM MPT tree and their alternative way of passing `nodes` in the ExecutionWitness](https://hackmd.io/@jsign/ethereum-zkvm-openvm-guest-input-and-optimized-mpt).

### M2 — Client implementations

#### Besu

No update

#### EthRex

- [Ivan] Execution witness pre-serialization optimization benchmarks — [ethrex#6058](https://github.com/lambdaclass/ethrex/pull/6058) (MERGED). 61% improvement over on-demand (242ms → 94ms).

#### Geth

No update

#### Nethermind

No update

#### Reth

No update

#### Zilkworm

No update

## Project 2: zkEVM Guest Program

### M1 — Finalize guest program in execution specs

No update

### Milestone Updates

### Ere + zkBoost

**Reproducible guest program ELF builds**
  * An automated test validating ELF reproducibility is available [here](https://github.com/eth-act/ere/commit/0e41aa7e95aa02df84ede0fcd04c1b0b45cfa090).

* **ELF signing with `minisign`**
  * Integrated `minisign` for signing guest program ELFs.
  * Introduced a dedicated binary for pulling and verifying ELFs using `minisign` ([commit](https://github.com/eth-act/zkboost/commit/636bea4e223d69de8df0d151cd3be39974371d3d)).

### Besu

No update

### EthRex

- [Ignacio] [Implemented full EIP-8025 support](https://github.com/eth-act/ere-guests/pull/7) in ere-guests.
- [Ivan] ELF distribution pipeline currently in progress: [ethrex#6019](https://github.com/lambdaclass/ethrex/pull/6019).
- [Ivan] Decoupled L1 and L2 guest programs into separate modules — [ethrex#5818](https://github.com/lambdaclass/ethrex/pull/5818).

### Geth

No update

### Nethermind

No update

### Reth

- [Ignacio] [Implemented full EIP-8025 support](https://github.com/eth-act/ere-guests/pull/7) in ere-guests.

### Zilkworm

No update

## Project 3: zkEVM Guest API standardization

### M1 — Standardize minimal hardware targets

- [Marcin] ["Zicclsm extension" proposal](https://github.com/eth-act/zkvm-standards/pull/9)
- [Marcin] Added [commits](https://github.com/eth-act/wasrisc/commits?author=marcinbugaj&since=2025-11-17&until=2026-01-26) to the [wasrisc](https://github.com/eth-act/wasrisc) repository to benchmark Go and Rust STF implementations when compiled through WebAssembly using `wasmtime`, `wasmer (cranelift/llvm)`, and other WASM frameworks. This exploratory work was related to the standardization effort - if WASM proves to be fast and usable, it could avoid the potentially upcoming "should we add atomics" discussion and would have supported staying with rv32im as the base instruction set. Furthermore, successful compilation through WASM would allow support for Go, C#, and other languages that have WASM backends but otherwise don't support bare metal systems as first-order citizens. Since the decision was made to use a bare metal system as the target platform, demonstrating the ability to compile through WASM was reassuring and ensured broader language compatibility.
- [Kevaundray] Added riscv64im-unknown-none-elf as a Tier 3 target into [Rust compiler](https://github.com/rust-lang/rust/pull/148790)

### M2 — Standardize zkVM precompiles via C headers

No update

### M3 — Standardize the interface for accessing IO via C headers

- [Marcin] ["zkVM IO interface" proposal](https://github.com/eth-act/zkvm-standards/pull/8)

### M4 — Standardize assumptions about the ELF and zkVM processing

- [Marcin] ["Memory Layout Restrictions" proposal](https://github.com/eth-act/zkvm-standards/pull/10)
- [Marcin] ["Host Build Support for Guest Programs" proposal](https://github.com/eth-act/zkvm-standards/pull/12)
- [Marcin] ["Standard Termination Semantics" proposal](https://github.com/eth-act/zkvm-standards/pull/14)

## Project 4: Consensus Layer Integration

### M1 — Finalize consensus specs

- [Francesco] Improved `make lint` in consensus-specs — [ethereum/consensus-specs#4838(MERGED)](https://github.com/ethereum/consensus-specs/pull/4838)

- [Francesco] Refactored EIP-8025 consensus specs — [ethereum/consensus-specs#4828(MERGED)](https://github.com/ethereum/consensus-specs/pull/4828)
- [Francesco] Execution-APIs PR: [ethereum/execution-apis#735(WIP)](https://github.com/ethereum/execution-apis/pull/735)
- [Francesco] Beacon-APIs PR: [ethereum/beacon-APIs#569(WIP)](https://github.com/ethereum/beacon-APIs/pull/569)

**Next:**

- Refactor EIP-8025 specs to align with additional modifications concluded from discussions and introduced in Lighthouse implementation
- Release initial version of specs

### M2 -- Spec out BiB changes

- [Kevaundray, Ignacio, Francesco] Added initial BiB (Block-in-Blobs) EIP — [ethereum/EIPs#11212(WIP)](https://github.com/ethereum/EIPs/pull/11212)
  - Deprecated BiB-related specs in favour of above formalised EIP:
    - [ethereum/execution-apis#736(DEPRECATED)](https://github.com/ethereum/execution-apis/pull/736)
    - [ethereum/consensus-specs#4847(DEPRECATED)](https://github.com/ethereum/consensus-specs/pull/4847)

### M3 -- Client implementations

#### Lighthouse

- [Francesco] Working on implementation of the refactored specs on [feat/eip8025(WIP)](https://github.com/eth-act/lighthouse/tree/feat/eip8025) branch

**Next:**

  - Networking integration, unit tests, Kurtosis E2E tests

#### Prysm

- [developeruche, syjn99] Initial work: [OffchainLabs/prysm#16003](https://github.com/OffchainLabs/prysm/pull/16003)
- [developeruche, syjn99] Continuation in Offchain Labs Prysm repository: [OffchainLabs/prysm#16270](https://github.com/OffchainLabs/prysm/pull/16270)

Implemented so far:
- Execution proof via specific ENR field
- Dummy proof production
- Proof gossiping on a specific subnet (broadcast/receive)
- RPC proof by root handler / requester
- Proofs storage on disk (quite similar to data column sidecars)
- Block importation rule modified if choosing to relying on execution proofs

- [syjn99] Ethproofs call #7 Demo branch: [syjn99/prysm ethproofs-demo](https://github.com/syjn99/prysm/commits/ethproofs-demo/), including:
  - Separate the prover from the beacon node
  - Accept execution proofs via Beacon API (POST /eth/v1/beacon/pool/execution_proofs)
  - [Source](https://discord.com/channels/595666850260713488/1457344719876260084/1465091645128315054)

**Next:**

  - During the next week, these 2 branches will be merged.

#### Grandine

No update

#### Lodestar

No update

#### Nimbus

No update

#### Teku

No update

## Project 5: Prover Infrastructure

### M1 — Integrate zkVMs into Ethproofs

No update

### M2 — Ensure GPU implementations are open source

No update

### M3 — Integrate zkVMs into Ere

- [Han] [Exploration integrating ZisK's cluster](https://github.com/eth-act/ere/compare/han/feature/zisk-network-prover) and [plan](https://github.com/eth-act/ere/issues/277) to integrate it into Ere

### M4 — Test zkboost in isolation with single and then multiple GPUs

- [Add E2E integration test for zkboost](https://github.com/eth-act/zkboost/pull/8)

### M5 — Metrics to track prover reliability and pipelining inefficiencies

- [Add Prometheus metrics and Grafana dashboard for zkboost](https://github.com/eth-act/zkboost/pull/15)

### M6 — Allow attesters to use this infrastructure to verify proofs

- [Example of running local testnet with zkboost and EWS (execution witness sentry)](https://github.com/eth-act/zkboost/pull/30)

## Project 6: Benchmarking and Metrics

### M1 — Benchmark guest programs against zkVMs locally

- [Ignacio] Zisk GPU scaling exploration in [this notebook](https://hackmd.io/@jsign/eth-zkvm-zisk-gpu-scaling).
- [Stefan, Han] Investigated further NUMA setup for Zisk which can explain bottlenecks in the document above.

### M2 — Integrate metrics into pandaOps' lab

No update

### M3 — Investigate multidimensional metering/pricing

- Repricings for block proving [Part 1](https://zkevm.ethereum.foundation/blog/repricings-for-block-proving-part-1) and [Part 2](https://zkevm.ethereum.foundation/blog/repricings-for-block-proving-part-2) were published.

## Project 7: Security

### M2 — Figure out what components need specifications

- [Cody] A work in progress uses AI to extract a [Python implementation](https://github.com/codygunton/pil2-proofman/tree/python-spec) of ZisK's proof system. This will give a concrete data point for the conversation on how specs should look.

### M3 — Figure out what the end-game for specs looks like

- [Cody] A work in progress uses AI to extract a [Python implementation](https://github.com/codygunton/pil2-proofman/tree/python-spec) of ZisK's proof system. This will give a concrete data point for the conversation on how specs should look.

### M5 — Establish a minimal go/no-go framework

- [Cody] A [blog post](https://zkevm.ethereum.foundation/blog/zkevm-security-overview) on the question of L1 zkEVM security from the holistic point of view.
- [Cody] [Merged:](https://github.com/eth-act/zkevm-test-monitor/pull/11) continuous monitoring of Pico for RISC-V compliance. Current compliance is 46/47 RV32IM architecture tests.
- [Cody] [Under review:](https://github.com/matter-labs/zksync-airbender/pull/175) RISC-V compliance testing for Airbender. Current compliance: 42/47 RV64IM architecture tests.
