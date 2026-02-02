# Update 1

> Please also link updates to the milestones in the planning doc

## Project 1: Execution Witness

Besu

EthRex

Geth

Nethermind

Reth

Zilkworm

Specs:
- Peter from STEEL created a [draft state tracker PR](https://github.com/ethereum/execution-specs/pull/2063). This will be required for execution witness generation.
- Ignacio [built on top for also tracking bytecodes and ancestor headers](https://github.com/petertdavies/execution-specs/pull/5) would look like, since this is also required for execution witness generation.
- Ignacio implemented an [e2e execution witness generation](https://github.com/jsign/execution-specs/pull/1) in the spec. It has its own state tracker that we could replace with Peter's state tracker, but also has many new components required for execution witness generation (e.g. MPT pre-state and branch colapse node detection, integration with testing framework, etc). Main goal is to help discussing how to move forward in steps in the spec.
- Ignacio wrote a deep dive into [OpenVM MPT tree and their alternative way of passing `nodes` in the ExecutionWitness](https://hackmd.io/@jsign/ethereum-zkvm-openvm-guest-input-and-optimized-mpt).

## Project 2: zkEVM Guest program
### Milestone Updates
- Reproducible guest program ELF build using Docker (dockerized `ere-compiler-*zkvm`)
  - A test for ELF reproducibility can be found [here(https://github.com/eth-act/ere/commit/0e41aa7e95aa02df84ede0fcd04c1b0b45cfa090)]
- Use `minisign` to sign ELFs
  - Introduced a [Binary](https://github.com/eth-act/zkboost/commit/636bea4e223d69de8df0d151cd3be39974371d3d) for `pulling` and `verifying` ELFs using `minisign`
- Complete ELF distribution pipeline
  - Ethrex ELF distribution pipeline is currently in progress handled by [Ivan](https://github.com/ivan) at his [PR](https://github.com/lambdaclass/ethrex/pull/6019)

## Project 2: zkEVM Guest Program

### Milestone Updates

* **Reproducible guest program ELF builds**
  * An automated test validating ELF reproducibility is available [here](https://github.com/eth-act/ere/commit/0e41aa7e95aa02df84ede0fcd04c1b0b45cfa090).

* **ELF signing with `minisign`**
  * Integrated `minisign` for signing guest program ELFs.
  * Introduced a dedicated binary for pulling and verifying ELFs using `minisign` ([commit](https://github.com/eth-act/zkboost/commit/636bea4e223d69de8df0d151cd3be39974371d3d)).

* **ELF distribution pipeline**
  * The Ethrex ELF distribution pipeline is currently in progress.
  * Implementation is being handled by [Ivan](https://github.com/ilitteri) in this PR: [https://github.com/lambdaclass/ethrex/pull/6019](https://github.com/lambdaclass/ethrex/pull/6019).

Besu

EthRex
- Ignacio [implemented full EIP-8025 support](https://github.com/eth-act/ere-guests/pull/7) in ere-guests.

Geth

Nethermind

Reth
- Ignacio [implemented full EIP-8025 support](https://github.com/eth-act/ere-guests/pull/7) in ere-guests.

Zilkworm

## Project 3: zkEVM Guest API standardization

### Marcin Bugaj's contribution

The following proposals:
- ["zkVM IO interface" proposal](https://github.com/eth-act/zkvm-standards/pull/8)
- ["Zicclsm extension" proposal](https://github.com/eth-act/zkvm-standards/pull/9)
- ["Memory Layout Restrictions" proposal](https://github.com/eth-act/zkvm-standards/pull/10)
- ["Host Build Support for Guest Programs" proposal](https://github.com/eth-act/zkvm-standards/pull/12)
- ["Standard Termination Semantics" proposal](https://github.com/eth-act/zkvm-standards/pull/14)

That involved discussions with stakeholders in the aforementioned PRs accompanied by discussions in the [[public] L1 zkEVM](https://web.telegram.org/k/#-2536148632) Telegram channel and in [Discord thread 1](https://discord.com/channels/595666850260713488/1463870144819364025) and [Discord thread 2](https://discord.com/channels/595666850260713488/1463487192411148351).

Additionally, [the following commits](https://github.com/eth-act/wasrisc/commits?author=marcinbugaj&since=2025-11-17&until=2026-01-26) were added to the [wasrisc](https://github.com/eth-act/wasrisc) repository to benchmark Go and Rust STF implementations when compiled through WebAssembly using `wasmtime`, `wasmer (cranelift/llvm)`, and other WASM frameworks. This exploratory work was related to the standardization effort - if WASM proved to be fast and usable, it could avoid the potentially upcoming "should we add atomics" discussion and would have supported staying with rv32im as the base instruction set. Furthermore, successful compilation through WASM would allow support for Go, C#, and other languages that have WASM backends but otherwise don't support bare metal systems as first-order citizens. Since the decision was made to use a bare metal system as the target platform, demonstrating the ability to compile through WASM was reassuring and ensured broader language compatibility.

## Project 4: Consensus Layer Integration

### Francesco Risitano's contribution

**Milestone 1 — Implement modifications according to the reference specification:**
- Working on implementation of the refactored specs on [feat/eip8025(WIP)](https://github.com/eth-act/lighthouse/tree/feat/eip8025) branch
- Next: networking integration, unit tests, Kurtosis E2E tests
- Optimistic target: preliminary demo ready for breakout call

**Milestone 2 — Port modifications in consensus-specs:**
- Refactored EIP-8025 consensus specs — [ethereum/consensus-specs#4828(MERGED)](https://github.com/ethereum/consensus-specs/pull/4828)
- Execution-APIs PR: [ethereum/execution-apis#735(WIP)](https://github.com/ethereum/execution-apis/pull/735)
- Beacon-APIs PR: [ethereum/beacon-APIs#569(WIP)](https://github.com/ethereum/beacon-APIs/pull/569)
- Next: Refactor EIP-8025 specs to align with additional modifications concluded from discussions and introduced in Lighthouse implementation

**Output 3 — Design strategy documents & EIPs:**
- Reviewed BiB (Block-in-Blobs) EIP — [ethereum/EIPs#11212(WIP)](https://github.com/ethereum/EIPs/pull/11212)
- Deprecated BiB-related specs in favour of above formalised EIP:
  - [ethereum/execution-apis#736(DEPRECATED)](https://github.com/ethereum/execution-apis/pull/736)
  - [ethereum/consensus-specs#4847(DEPRECATED)](https://github.com/ethereum/consensus-specs/pull/4847)

**Misc:**
- Improved `make lint` in consensus-specs — [ethereum/consensus-specs#4838(MERGED)](https://github.com/ethereum/consensus-specs/pull/4838)
- Gained familiarity with specs, objectives, tools and misc repos relevant to EIP-8025
- Next: Prepare slides for breakout call

### Prysm

Initial work done by [developeruche](https://github.com/developeruche) and [syjn99](https://github.com/syjn99):
- https://github.com/OffchainLabs/prysm/pull/16003

Continuation moved in a branch directly in the Offchain Labs Prysm repository:
- https://github.com/OffchainLabs/prysm/pull/16270

Implemented so far:
- Execution proof via specific ENR field
- Dummy proof production
- Proof gossiping on a specific subnet (broadcast/receive)
- RPC proof by root handler / requester
- Proofs storage on disk (quite similar to data column sidecars)
- Block importation rule modified if choosing to relying on execution proofs

[syjn99](https://github.com/syjn99) has a branch:
- https://github.com/syjn99/prysm/commits/ethproofs-demo/

that will be used for a demo including:
- Separate the prover from the beacon node
- Accept execution proofs via Beacon API (POST /eth/v1/beacon/pool/execution_proofs)

[Source](https://discord.com/channels/595666850260713488/1457344719876260084/1465091645128315054)

During the next week, these 2 branches will be merged.

### Grandine

### Lodestar

### Nimbus

### Teku

## Project 5 Prover Infrastructure

### Milestones Updates

- Integrate zkVMs into Ere
  - [Exploration integrating ZisK's cluster](https://github.com/eth-act/ere/compare/han/feature/zisk-network-prover) and [plan](https://github.com/eth-act/ere/issues/277) to integrate it into Ere
- Test zkboost in isolation with a single and then multiple GPUs
  - [Add E2E integraton test for zkboost](https://github.com/eth-act/zkboost/pull/8)
- Allow attesters to use this infrastructure to verify proofs
  - [Example of running local testnet with zkboost and EWS (execution witness sentry)](https://github.com/eth-act/zkboost/pull/30)
- Metrics to track prover reliability and pipelining inefficiencies
  - [Add Prometheus metrics and Grafana dashboard for zkboost](https://github.com/eth-act/zkboost/pull/15)

## Project 6: Benchmarking and Metrics

- Ignacio did a Zisk GPU scaling exploration in [this notebook](https://hackmd.io/@jsign/eth-zkvm-zisk-gpu-scaling).
- Stefan and Han investigated further NUMA setup for Zisk which can explain bottlenecks in the document above.
- Repricings for block proving [Part 1](https://zkevm.ethereum.foundation/blog/repricings-for-block-proving-part-1) and [Part 2](https://zkevm.ethereum.foundation/blog/repricings-for-block-proving-part-2) were published.

## Project 7: Security

M2 and M3:
 - A work in progress uses AI to extract a [Python implementation](https://github.com/codygunton/pil2-proofman/tree/python-spec) of ZisK's proof system. This will give a concrete data point for the conversation on how specs should look.

M5: 
 - A [blog post](https://zkevm.ethereum.foundation/blog/zkevm-security-overview) on the question of L1 zkEVM security from the holistic point of view. 
 - [Merged:](https://github.com/eth-act/zkevm-test-monitor/pull/11) continuous monitoring of Pico for RISC-V compliance. Current compliance is 46/47 RV32IM architecture tests.
 - [Under review:](https://github.com/matter-labs/zksync-airbender/pull/175) RISC-V compliance testing for Airbender. Current compliance: 42/47 RV64IM architecture tests.
