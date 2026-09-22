# Promise to Proof + Atomic

This repository specifies an integration between [Promise to Proof](https://github.com/grove/promise-to-proof) and [Atomic](https://github.com/bastani-inc/atomic). Promise to Proof defines how agents turn a request into an acceptance contract, implement it, review the result, and prove each promised outcome. The proposed integration uses Atomic to run those stages as a durable, resumable workflow.

## Status

The integration is proposed, not implemented. The version 1 implementation specification is a draft proposed for approval. This repository contains design documents and a JSON Schema; it does not contain a runnable workflow or setup commands.

## Planned version 1

A run handles one unsliced work item in one repository. It plans and saves an approved acceptance contract, implements in an isolated workspace, then runs separate review and proof against the same candidate. The workflow can route bounded review corrections and eligible proof repairs, and it stops for human decisions that change the agreement or authorize consequential actions.

Version 1 does not cover slicing work into child items, CI repair, commits, pushes, pull requests, merges, or deployment. It does not copy implementation changes over the operator's source checkout.

## Project documents

- [Atomic integration overview](docs/atomic-integration.md) explains how the two projects fit together and sketches possible extensions.
- [P2PA-001 implementation specification](docs/spec/p2pa-001.md) defines proposed version 1 requirements, workflow behavior, and release gates.
- [P2PA-001 contracts schema](docs/spec/p2pa-001.contracts.schema.json) defines the structural contracts for workflow inputs, phase results, and final handoffs. The specification also requires semantic validation beyond the schema.

## License

This project is licensed under the [Apache License 2.0](LICENSE).
