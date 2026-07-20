# Full Fulu Compatibility and Cross-Client Interoperability for Ream

End-to-end beacon-chain, PeerDAS, custody, and data-availability interoperability for the Ream consensus client.

## Motivation

The [**Fulu upgrade**](https://ethereum.org/roadmap/fusaka/) introduces PeerDAS, allowing nodes to store and serve only a portion of the blob data instead of requiring every node to store the full data. It also introduces deterministic proposer lookahead and configurable blob schedules, requiring updates across beacon state transition, block validation, proposer selection, and consensus-layer networking.

A Fulu-compatible client must support both beacon-chain and PeerDAS interoperability: processing blocks, attestations, and fork-choice updates while also validating, gossiping, requesting, reconstructing, and serving data columns.

This project began with the [**Ream: Minimal PeerDAS Data Availability Client proposal**](https://github.com/eth-protocol-fellows/cohort-seven/blob/main/projects/project-ideas.md). After taking a closer look at Ream, we found that its beacon client still has several gaps and mismatches with the consensus specifications, as well as limited end-to-end and cross-client test coverage. E.g: even running two Ream nodes together does not yet work reliably and still exposes errors.

This project therefore aims to complete Ream’s beacon-client implementation, add full PeerDAS support, and make Ream interoperable with mature clients such as Lighthouse and Prysm.

## Project description

We will implement and validate Fulu interoperability in Ream in six phases:

1. **Fulu specification gap analysis**: compare Ream's beacon-client implementation against the Fulu consensus specifications, identify missing or incorrect functionality which were described on spec, and produce a concrete implementation and testing backlog.

2. **Ream multi-node end-to-end testing**: implement end-to-end tests for networks containing multiple Ream beacon nodes, identify and fix protocol and implementation issues, and demonstrate that the nodes can reliably process blocks, agree on the canonical chain, and reach finality.

3. **Kurtosis and cross-client interoperability**: integrate Ream into a Kurtosis-based Ethereum network and run it alongside mature consensus clients such as Lighthouse and Prysm. Use these networks to identify, report, and fix incompatibilities across genesis, peering, synchronization, block and attestation processing, fork choice, and finality.

4. **PeerDAS full-custody mode and DA decoupling**: implement a PeerDAS-compliant full-custody mode in which Ream stores and serves the complete blob dataset as data columns. Decouple data-availability responsibilities from the beacon-chain core behind explicit interfaces for column validation, storage, retrieval, reconstruction, serving, and availability tracking.

5. **PeerDAS custody mode**: extend the full-custody implementation to support configurable custody groups. A Ream node must derive its assignments and retrieve, verify, persist, reconstruct, and serve the columns required by its configured custody mode.

6. **Standalone DA node (optional)**: if Ream still benefits from a separate DA client after the beacon and DA boundaries are stabilized, extract the data-availability subsystem into a standalone process or integrate it with related standalone DA work.

## Specification

### Fulu specification alignment

- Compare Ream against the Fulu consensus specifications.
- Identify and implement missing or incorrect beacon-client functionality.

### End-to-end testing

- Run multiple Ream nodes in the same network.
- Fix issues preventing synchronization, block production, and finality.

### Cross-client interoperability

- Integrate Ream with Kurtosis.
- Test beacon-chain and PeerDAS interoperability with Lighthouse and Prysm.

### Full-custody PeerDAS

- Store and serve the complete blob dataset as PeerDAS data columns.
- Decouple column validation, storage, retrieval, and availability tracking from the beacon-chain core.

### Custody mode

- Support configurable custody groups and subnet subscriptions.
- Retrieve, verify, reconstruct, store, and serve assigned columns.

### Standalone DA node (stretch)

- Extract the DA subsystem into a standalone process if it remains useful after decoupling.

## Roadmap

The phases are ordered by dependency, with some overlap between testing, interoperability, and PeerDAS implementation.

**Phase 1 - Fulu specification gap analysis (weeks 0-4)**

- Compare Ream with the Fulu consensus specifications.
- Identify missing or incorrect beacon-client functionality.
- Create an implementation and testing backlog.

**Phase 2 - Multi-node Ream beacon network (weeks 5-9)**

- Implement end-to-end tests for running multiple Ream beacon nodes.
- Identify and fix synchronization, block-processing, mis-matched in old specs and networking issues.
- Ensure the network can reliably produce and finalize blocks.

**Phase 3 - Kurtosis and cross-client interoperability (weeks 6-10) - Parallel with Phase 2**

- Integrate and launch Ream with Kurtosis.
- Run Ream alongside Lighthouse and Prysm.
- Identify and fix beacon-chain and networking incompatibilities.

**Phase 4 - Full-custody PeerDAS and DA decoupling (weeks 11-14)**

- Store and serve the complete blob dataset as PeerDAS data columns.
- Implement data-column gossip and request/response protocols.
- Separate DA validation, storage, retrieval, and availability tracking from beacon logic.
- PeerDas compatibility when running Kurtosis on PeerDAS tests.

**Phase 5 - PeerDAS custody mode (weeks 15-16)**

- Implement custody-group calculation and subnet management.
- Support configurable custody requirements.
- Test column retrieval, verification, reconstruction, storage, and serving across clients.

**Phase 6 - Hardening and standalone DA node (weeks 16-21+, stretch)**

- Fix remaining interoperability issues and add metrics and documentation.
- Making sure everything run smooth with Prysm and Lighthouse.
- Work on making a seperate standalone DA node if needed (Based on Ream's roadmap).

## Possible challenges

- Ream may have undocumented gaps or mismatches with the Fulu specifications.
- Cross-client failures can be difficult to reproduce and may depend on timing, networking, or configuration.
- Beacon-chain compatibility does not guarantee PeerDAS interoperability, which has separate gossip, custody, and request/response requirements.
- Decoupling DA from consensus-critical beacon logic must handle missing data, restarts, and partial failures safely.
- Full custody and reconstruction may require significant CPU, memory, bandwidth, and storage.

## Goal of the project

The goal is a Fulu-compatible Ream beacon client that can operate reliably in Ream-only and mixed-client networks.

Success criteria:

1. Multiple Ream nodes can synchronize, produce blocks, and reach finality.
2. Ream interoperates with Lighthouse and Prysm through Kurtosis.
3. Ream exchanges beacon-chain messages and PeerDAS data columns with other clients.
4. Ream supports both full-custody and configurable custody modes.
5. End-to-end tests cover beacon-chain and PeerDAS interoperability.
6. The DA subsystem is decoupled from the beacon-chain core and can support a future standalone DA node.

This establishes Ream as a Fulu-compatible client ready to participate in multi-client devnets.

## Collaborators

### Fellows

- **Daniel Pham** ([@perfogic](https://github.com/perfogic)):
- **Tosin** ([@tosynthegeek](https://github.com/tosynthegeek)):
- **Hans Vuong** ([@vuonghuuhung](https://github.com/vuonghuuhung)):

The workload will be shared equally among the fellows throughout the project.

### Mentors

- [Shariq](https://github.com/shariqnaiyer)

## Resources

- [Ream repository](https://github.com/ReamLabs/ream)
- [Ethereum consensus specifications](https://github.com/ethereum/consensus-specs)
- [Fulu consensus specifications](https://github.com/ethereum/consensus-specs/tree/master/specs/fulu)
- [PeerDAS specification](https://github.com/ethereum/consensus-specs/tree/master/specs/fulu)
- [Ethereum consensus networking specifications](https://github.com/ethereum/consensus-specs/tree/master/specs/phase0/p2p-interface.md)
- [Ethereum Kurtosis package](https://github.com/ethpandaops/ethereum-package)
- [Lighthouse](https://github.com/sigp/lighthouse)
- [Prysm](https://github.com/OffchainLabs/prysm)
- [Ream: Minimal PeerDAS Data Availability Client project idea](./project-ideas.md#ream-minimal-peerdas-data-availability-client)
