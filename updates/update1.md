# Update 1

> Please also link updates to the milestones in the planning doc

## Project 1: Execution Witness

Besu

EthRex

Geth

Nethermind

Reth

Zilkworm

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

Geth

Nethermind

Reth

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

Grandine

Lighthouse

Lodestar

Nimbus

Prysm

Teku


## Project 5: Prover Infrastructure

## Project 6: Benchmarking and Metrics