# zkEVM on L1

This document describes the set of milestones/sub-projects needed for Ethereum L1 to support zkEVM proofs as an alternative to re-execution in the attesting workflow.

A fundamental part of shipping zkEVMs on mainnet is to integrate back into L1's governance and infrastructure. While learning with our current infrastructure teams like pandaOps, EF Security, STEEL how the software/hardware stack works.

## High-Level End-to-End Flow

At a high level the pipeline for getting a proof to test attester looks like the following:

```java
EL client (stateful)
    
    ↓ produces

ExecutionWitness (stateless input)

    ↓ consumed by

zkEVM Guest Program (stateless validation)

    ↓ executed in

zkVM (RISC-V or other target)

    ↓ produces

zkEVM Proof of Execution

    ↓ consumed by

CL client (attester / verifier)
```

Below we describe sub-projects that make the above workflow possible.

## Project 1: ExecutionWitness

**Goal:** Produce the execution witness, a data structure that is produced per block holding all required data to validate the block.

**Owners:** EL clients

**Outputs:**
- ExecutionWitness format described in [execution-specs](https://github.com/ethereum/execution-specs) with tests.
    - This implies a method for generating them in the execution specs, allowing other clients to check that conformance.
- RPC endpoint described in [execution-apis](https://github.com/ethereum/execution-apis)
    - Currently we use `debug_executionWitness`. Tentatively, creating a new endpoint that is zk-optimized would be optimal here. This endpoint is currently being used in production by Optimism's [Kona](https://github.com/op-rs/kona).
- Optimizing the ExecutionWitness for the guest program
- RPC compatibility tests
- Implemented in EL clients
    - Clients should be able to generate the ExecutionWitness for the last $N$ blocks. $N$ here is related to how tolerant the EL is to reorgs.
    - Clients should conform to the specifications given above 

**Milestones:** https://hackmd.io/i4z1eOYATKq3Ang-9Zw5hQ

**Dependencies:**
- *Upstream*: execution-specs
    - Concretely, this is blocked by the ability for the execution specs to track what state has been accessed in the block. This gets added with Block Level Access Lists (BALs). As of November 2025, we have told the STEEL team that this is not a high enough priority to warrant them pulling it out of the BALs PR and applying it to previous forks. See tracking issue [here](https://github.com/ethereum/execution-specs/issues/1865).
- *Downstream*: Guest program
    - This is the non-trivial input for the program that gets proven. While we have optional proofs, this will predominantly be called via RPC.

**Working Group:**

- Somnath (Erigon)
- Pawel (Erigon)
- Ignacio (EF zkEVM)
- Peter (EF STEEL)
- Ben (Nimbus)
- Felix (Geth)
- Guillaume (Geth)
- Tanishq/Ruben/Nikita (Nethermind)
- Matt (Reth)
- Karim (Besu)

## Project 2: zkEVM Guest Program

**Goal:** Define the stateless validation logic that consumes a EL block + ExecutionWitness and determines whether there is a valid state transition function.

**Owners:** EL clients

**Outputs:**

- Stateless program described in [execution-specs](https://github.com/ethereum/execution-specs) with tests.
- Reproducible compilation workflow
- Compiles to a [standardized](https://github.com/eth-act/zkvm-standards) target
    - This process makes it explicitly clear, how many targets there are and what assumptions we are making about them. 

**Milestones:** https://hackmd.io/i4z1eOYATKq3Ang-9Zw5hQ

**Dependencies:**
- *Upstream:* ExecutionWitness stability (Project 1), 
- *Downstream:* zkVMs

**Working Group:**

- Somnath (Erigon)
- Pawel (Erigon)
- Ignacio (EF zkEVM)
- Peter (EF STEEL)
- Ben (Nimbus)
- Kim (Nimbus)
- Guillaume (Geth)
- Felix (Geth)
- Tanish (Nethermind)
- Matt (Reth)
- Gary (Besu)
- Karim (Besu)

## Project 3: zkVM-guest API Standardization

**Goal:** Standardize the interface between the zkVM and the guest program

**Owners:** EL and zkVM teams

**Outputs:**
- A standardized list of targets
- A standardized interface for accessing zkVM precompiles
- A standardized interface for accessing IO
- Codified assumptions; for example, alignment assumptions, assumptions on memory layout, trap semantics, bootloader, can the ELF have data in code, etc

**Milestones:**
- Standardize minimal hardware targets that:
    - Each EL can compile to
    - Each zkVM will target
- Standardize the zkVM precompiles available via C headers
- Standardize the interface for accessing IO via C headers
- Standardize assumptions made about the ELF and the zkVM's processing of the ELF

**Dependencies:** None

**Working Group:**

- Marcin (Lita)
- Kev (EF zkEVM)
- Alex Hicks (EF zkEVM)
- Guillaume (Geth)
- Maxim Menshikov (Nethermind)
- Jordi (Zisk)
- Alex Vlasov (Airbender)
- Tamir (Succinct)
- Jonathan (Axiom)
- Stephen (Ziren)
- Jeremy (RISC0)
- Michael (Jolt)
- Alan (Brevis)

# Project 4: Consensus Layer Integration

**Goal:** Allow CL clients to verify zkEVM proofs as part of beacon block validation.

**Outputs:**

- General document on how the CL is modified to accommodate for proofs
- Stable consensus-specs version
- Documents and EIPs on the different design strategies, eg blocks in blobs
- Document possible changes to [EIP-7870](https://eips.ethereum.org/EIPS/eip-7870) for attesters and full-nodes

**Milestones:**

- Implement modifications according to the reference specification
- Port modifications in consensus-specs
- All clients should use the consensus-specs and ensure that they are passing the relevant test vectors
- [Internal EF rollout + onboarding plan](https://hackmd.io/@kevaundray/H1x1K-ckWg)

**Dependencies:**
- Upstream: This depends on all other components

**Working Group:**

- Kev (EF zkEVM on Lighthouse)
- Pari (EF PandaOps)
- George (EF Cryptography research on proof size)
- Barnabas (EF PandaOps)
- Raul (EF Networking on *proof sizes*)
- Manu (Prysm)
- Jun Song + Uche (EPF on Prysm)
- Gabriel (Teku)
- Lucas (Teku)
- Dapplion (Lighhouse)
- Saulius (Grandine)
- Kim (Nimbus)
- Twoeth (Lodestar)

# Project 5: Prover infrastructure

**Goal:** Create the necessary infrastructure and interfaces to generate proofs locally and for an attester client to verify proofs.

**Owners:** EF and proving teams

**Milestones:**

- Integrate zkVMs into Ethproofs
- Ensure GPU implementations are open source
- Integrate zkVMs into Ere
- Test zkboost in isolation with a single and then multiple GPUs
    - This does not need to be with the stateless guest program as we mainly care about testing the infrastructure and not necessarily the stateless validation program
- Metrics to track prover reliability and pipelining inefficiencies
- Allow attesters to use this infrastructure to verify proofs

**Working Group:**

- Han (EF zkEVM)
- Pari (EF PandaOps)
- Stefan (EF PandaOps)
- Barnabas (EF PandaOps)
- Markus (EF Devops)
- Fara (EF Ethproofs)
- Kev (EF zkEVM)
- zkVM teams (For assistance on GPU orchestration and integration)

# Project 6: Benchmarking and Metrics

This a perpetual project.

**Goal:** Implement robust benchmarking mechanisms that can be used for future network analysis and gas repricing

**Owners:** EF

**Outcomes:**

- Document explaining the items we want to benchmark/gather metrics for in:
    - Guest programs: eg opcodes being used, native cycle count and its relationship to proving time
    - CL: eg proof propagation time
    - EL: eg witness generation time 
    - zkVM: eg proof creation and verification time, relationship between number of GPUs and proof time
- Projected opcode repricings for zk
    - Including draft EIPs for adjusting the price of certain opcodes
- Define prover hardware requirements
    - This is for a particular gas limit
- Benchmark and document verification time using [EIP-7870](https://eips.ethereum.org/EIPS/eip-7870)

**Milestones:**

- Benchmark all available guest programs against all available zkVMs locally (single then multi)
- Integrate metrics into [pandaOps' lab](https://lab.ethpandaops.io/)

**Working Group:**

- Ignacio (EF zkEVM)
- Han (EF zkEVM)
- Jochem (EF Prototyping)
- Fara (EF Ethproofs)
- Marius (EF Geth)
- Maria (EF RIG)
- Pawel (Erigon)
- Pari (EF PandaOps)
- Barnabas (EF PandaOps)

# Project 7: Security

This a perpetual project.

**Goal:** Establish security metrics for all of the different components; guest program, zkVMs, GPU orchestration, consensus layer client. Set up monitoring of these metrics and champion hardening initiatives.

**Owners:** ELs, CLs, zkVM teams, EF

**Outputs:**
- Specifications for the relevant proof system components
- zkVM that follows the security guidelines and proof sizes in EF cryptography research blogpost [TODO: INSERT LINK WHEN PUBLISHED]
    - This will have provable security as a requirement
- Create Fork readiness review document
    - This includes decisions on whether we will need bug bounties and audits for optional proofs
- Formal verification:
    - Document on what components we want to be formally verified for optional proofs
    - Tracker for which zkVM/EL is formally verified 
- Continuous threat modeling
- Documents on prover incentives and the decision tree for needing it in-protocol
- Dashboard for monitoring coverage of testing, auditing, fuzzing and formal verification.
- Documented security model and trust assumptions for each component (guest program, zkVM, prover infra, CL/EL, recursion) and overall system
- Supply-chain security requirements:
    - Reproducible builds and artifact signing for guest programs, zkVMs, and proof-critical binaries
- Incident-response and rollback runbook for proof-related issues
- Differential testing & fuzzing plan for zkVMs and guest programs
- GPU security requirements (side channels, isolation, driver risks, multi-tenant constraints)
**Milestones:**

- Agree on proof size, security regime and timelines
- Figure out what proving system components need specifications
    -  Specs on the structure of each zkVMs proving system
    -  Specs for the recursion topology
    -  Paper-to-code algorithmic specs
    -  Precise security proofs on the entire SNARK
- Figure out what the end-game for specs looks like and what the value in having teams write specs in the short term will be
- Figure out guarantees around each component; for example:
    - Can the guest program panic on invalid input?
    - Will the verifier panic when given an invalid proof?
    - Is the program assumed to be trusted?
- Establish a minimal go/no-go framework for recommending the use of ZKEVMs for scaling. Some possible metrics:
    - Test coverage
    - Audit coverage
    - Bug bounties
    - Stable RTP for some amount of time
    - Software diversity
    - Formal verification
    - Fuzzing
- Identify the minimum acceptable formal verification requirement for each of the following components:
    - Guest program
    - Target specs
    - SNARK prover
    - SNARK verifier

**Working Group:**
- Alex Hicks (EF snarkification)
- George (EF cryptography research)
- Arantxa (EF cryptography research) 
- Fredrik (EF Protocol Security)
- Marius (EF Geth) -- for guest program security
- Kev (EF zkEVM)
- Cody (EF zkEVM)
- Marcin (Lita)
- zkVM teams for speccing
- Pari (EF PandaOps)
- Barnabas (EF PandaOps)
- Julian (RIG)

# Project 8: ePBS (External)

**Goal:** To increase the amount of time available to prove a block.

**Owners:** CL devs, EF security

**Note:** This is not a project that we are working on. However, it is an optimization that we need. Without this feature, the amount of time the prover has to create a proof is 1-2 seconds. With this, the amount of time is 6-9 seconds.

**Timeline:** This will be deployed in Glamsterdam. Glamsterdam is expected to be mid 2026.

**Main ask:** We would like to know when ePBS is stable enough so that we can start rebasing work on top of it.

**Working Group:**

- Potuz (Prysm)
- Terence (Prysm)
- Justin Traglia (EF security)
- Mark (Lighthouse)
- Shane (EPF on Lighthouse)
- Saulius (Grandine)
- Nico (Lodestar)
- Enrico (Teku)
- Stefan (Teku)
- Caleb (Nimbus)

**Dependencies:**
*Downstream:* Consensus Layer integration
- Without this, the consensus layer attester will always be attesting late

# FAQ on interactions

- How will zkVM teams get the source code to make proofs for a specific EL? (Currently some use RSP)
    - I would suggest that we add a link to the zkevm.ethereum.org site for each EL. 
- Where does the CL get the ELF file from?
    - Nominally, from the release page of the chosen EL.
- Where does the EL find the zkVM C header files for interop?
    - zkVM teams will need to document where this is located. I propose that we put it in the zkvm-standards repo, next to the actual standard.
- Where do zkVM teams find the ELF files or source files for each EL, for local testing?
- Where does security find the specs for a zkEVM?
    - This has not been created, though I suggest we also put this on ethproofs and zkevm.ethereum.org
- What happens if an upgrade is needed between hardforks? (minor patch and critical bug fix)
    - https://hackmd.io/@kevaundray/B1dE392-Ze
- How do I know what should be a private input and what should be public?
    - This will be specified in consensus-specs.