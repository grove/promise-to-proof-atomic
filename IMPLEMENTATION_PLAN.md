# P2PA-001 Implementation Runbook

This is a human-operated runbook. Run each prompt in a fresh session where directed, inspect every returned artifact, replace angle-bracketed placeholders with exact saved references, and perform each approval or publication step yourself.

Use Promise to Proof at **two levels**: first to turn the integration specification into a parent acceptance contract and decompose it into independently deliverable children; then run the normal `plan → implement → review + prove` cycle for every child. Because the specification requires changes in both Atomic and the integration repository, do **not** try to implement it as one giant `implement-contract` run.

This decomposition organizes the project that builds version 1. It is not a feature of the delivered workflow. The implementation must still accept one unsliced work item and reject parent/child workflow inputs as required by P2PA-001 section 3.

I would use the following process.

### 0. Make the specification the durable source

Before invoking any skill, put the specification and schema somewhere every subsequent agent session can read. I’ll assume:

```text
docs/specs/p2pa-001.md
docs/specs/p2pa-001.contracts.schema.json
```

in the repository that will own the integration. Keep the specification itself unchanged while implementation is underway; requirement changes should go through the contract amendment process.

Use a persistent artifact location outside the candidate repository, for example:

```text
~/.local/state/promise-to-proof/p2pa-001/
```

The key principle is that the **specification remains the source**, the **acceptance contract becomes the executable agreement**, and implementation reports/proofs remain separate artifacts.

## 1. Create the parent acceptance contract

Run `plan-acceptance` against the specification, not against an implementation proposal.

Copy/paste:

```text
/plan-acceptance docs/specs/p2pa-001.md

Treat docs/specs/p2pa-001.contracts.schema.json as normative companion
material to the specification.

Build the parent acceptance contract for P2PA-001.

Requirements:
- Preserve every normative MUST and MUST NOT in the specification.
- Reconcile the explicit R1-R25 acceptance requirements with the contract;
  do not create a second competing checklist.
- Preserve the distinction between Atomic responsibilities, Promise-to-Proof
  skill responsibilities, integration responsibilities, and operator authority.
- Treat sections 2.1 and 19-20 as binding requirements, not implementation suggestions.
- Keep the contract candidate-independent.
- Do not weaken requirements because the current Atomic API lacks a capability.
- Do not invent implementation details unless a binding invariant requires them.
- Record genuine unresolved outcome decisions as open questions rather than
  guessing.
- Use stable requirement IDs and retain traceability to the specification.
- Include a traceability appendix mapping every normative statement in the
  specification to one or more acceptance requirement IDs. The appendix is an
  index, not a second checklist; no normative statement may be unmapped.
- The implementation must ultimately satisfy the complete v1 release, not only
  Gate A or the direct happy path.

Canonical contract destination:
docs/acceptance-contracts/p2pa-001.md

Do not implement anything. Return the proposed contract and storage handoff only.
```

Before approving the contract, compare its traceability appendix with the specification. Confirm that R1-R25 appear exactly once in the acceptance matrix, every normative statement is mapped, no source obligation was invented, and no outcome-defining question remains unresolved. Revise the proposal if any check fails.

Then save the exact returned bytes to:

```text
docs/acceptance-contracts/p2pa-001.md
```

and reread them. That saved parent contract is now the agreement all subsequent work refers to.

## 2. Slice the parent contract

This specification is clearly too large and cross-cutting for one coherent implementation session. Use `slice-contract` before writing code.

```text
/slice-contract docs/acceptance-contracts/p2pa-001.md; draft only

Use docs/specs/p2pa-001.md and its companion schema as the source context.

Decompose the parent contract into the fewest independently useful implementation
outcomes.

Important constraints:
- Respect repository boundaries. Work requiring changes to bastani-inc/atomic
  must not be mixed into the same implementation ticket as integration code in
  the Promise-to-Proof/integration repository.
- Treat section 20's Gate A, Gate B, Gate C, and Gate D as the intended dependency
  order, but do not mechanically create one ticket per heading if a different
  minimal decomposition gives better independently verifiable outcomes.
- The Atomic persistenceMode capability is an explicit prerequisite and should
  be represented as its own deliverable or external prerequisite.
- Every parent requirement R1-R25 must have an accountable contribution and
  final completion check.
- Cross-cutting requirements such as host compatibility, exact identity,
  authorization, and final parent verification must remain visible.
- Do not treat passing child proofs as proof of the parent contract.
- Final parent review and proof must run against the complete release subject
  defined below.
- Draft only. Do not create issues or modify trackers yet.
```

I would expect the result to look roughly like this, although `slice-contract` should make the final decision:

| Likely child                                  | Outcome                                                                                                           |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **A1 — Atomic durable-mode contract**         | Atomic exposes the selected `persistenceMode` correctly in fresh/resumed/headless/interactive workflow execution. |
| **A2 — Integration foundation and isolation** | Pinned skills, schemas, role-bound tools, grants, durable admission, artifact/workspace machinery.                |
| **B — Direct vertical path**                  | Source → contract → implementation → review/proof → `VERIFIED`, without corrections.                              |
| **C — Controlled corrections**                | Review correction, proof repair, amendment routing, verification epochs and budgets.                              |
| **D — Recovery and operational release**      | Resume, effect reconciliation, interrupted mutation, cancellation, ownership, complete release conformance.       |

A1 should live in the Atomic repository. The remaining integration children should live in this integration repository unless the draft identifies a concrete repository-owned prerequisite.

Do not invent a synthetic candidate snapshot spanning both repositories. The final release subject has these separately recorded identities:

- The **integration candidate** is one exact recoverable snapshot from this repository, with one fixed comparison base. This is the parent review/proof candidate.
- The **Atomic host prerequisite** is the exact proven A1 candidate or build, identified by its full source identity and reproducible build digest. It is part of the tested environment, not part of the integration candidate snapshot.
- The **skill bundle** is the exact retained Promise to Proof bundle from the source baseline in P2PA-001 section 2.

After A1 is complete, every dependent child must use the same pinned Atomic host prerequisite. A change to that prerequisite makes dependent compatibility and end-to-end evidence stale.

## 3. Approve and publish the decomposition

After inspecting the draft, explicitly approve it. If you're using GitHub Issues as the tracker, the prompt would be:

```text
/slice-contract docs/acceptance-contracts/p2pa-001.md

Publish the approved decomposition.

Preserve the exact parent contract identity and the approved coverage map.
Create child records only in destinations I have authorized.

For work in the integration repository, publish the approved child issues there.
For the Atomic host prerequisite, publish into bastani-inc/atomic only if that
destination is explicitly authorized; otherwise record it as an external
prerequisite with its complete proposed child source material.

Preserve qualified parent requirement references in every child.
Do not implement any child.
```

After publication, each child issue/specification is only **source material**. It does not yet have an acceptance contract.

That distinction matters.

# 4. Plan acceptance separately for every child

Do this for every published child.

For example, for the Atomic host capability:

```text
/plan-acceptance <Atomic-host-child-reference>

Create the child acceptance contract for this implementation unit.

Read the P2PA-001 parent contract, exact parent snapshot, decomposition, and this
child's qualified contribution.

This child must deliver only its assigned contribution while preserving all
applicable parent invariants.

In particular:
- Do not broaden this into implementation of the full Promise-to-Proof workflow.
- Preserve the parent's durable-only semantics.
- Define observable evidence for persistenceMode in durable and memory modes.
- Cover fresh, resumed, interactive, and headless workflow execution where the
  parent requires them.
- Preserve current Atomic API compatibility except where the approved child
  explicitly changes it.
- Map every child requirement back to its qualified parent obligation.

Return a candidate-independent contract. Do not implement.
```

Then repeat that pattern for A2, B, C, and D.

The important part is: **every child gets its own contract before implementation starts**.

# 5. Implement a child

Once a child contract is saved and ready, use `implement-contract`.

For the Atomic prerequisite, for example:

```text
/implement-contract <saved Atomic-host child contract>

Implement the complete child contract and nothing outside its spec envelope.

Use the checked Atomic source baseline as the starting point.

Preserve the existing workflow architecture. Implement the public selected-backend
capability required by the contract rather than inferring durability from
configuration, warnings, or database health.

Add the smallest complete implementation and meaningful tests required by the
contract. Cover all specified execution modes and resume behavior.

Do not implement the Promise-to-Proof integration itself in this child.
Do not weaken the contract because an existing internal API is inconvenient.
Do not commit, push, create a PR, or publish anything.

Run appropriate focused and broader development checks.
Capture the exact resulting candidate and save the implementation handoff under:

~/.local/state/promise-to-proof/p2pa-001/<child-id>/implementation/

Return the implementation report and exact recoverable candidate.
```

For an integration child, the same pattern applies:

```text
/implement-contract <saved child contract>

Implement the complete child contract in the managed scope.

Read the parent P2PA-001 contract, this child contract, the decomposition, and
all prerequisite outcome handoffs.

Implement the smallest complete solution. Preserve:
- exact agreement and candidate identity rules,
- role and authorization boundaries,
- structured result validation,
- artifact/report separation,
- durable workflow behavior,
- explicit failure rather than optimistic fallback.

Do not implement later child outcomes unless they are strictly necessary for
this child's promised behavior.

Run the contract-grounded development checks.
Do not commit, push, publish, merge, or deploy.

Capture and return the exact candidate and implementation handoff.
```

# 6. Independently review the candidate

Use a **fresh invocation/session**.

```text
/review-implementation <saved implementation handoff> against <fixed comparison base>

Review this exact candidate against its saved child contract and applicable
P2PA-001 parent obligations.

Use the fixed comparison base from the handoff.

Examine:
- contract fidelity,
- scope and simplicity,
- engineering quality,
- binding security and authorization constraints,
- whether the implementation actually enforces boundaries rather than merely
  describing them in prompts,
- whether deterministic protocol rules are implemented deterministically.

Do not repair anything.
Do not perform acceptance proof.
Do not treat development checks or green CI as acceptance.

Return REVIEWED, CHANGES NEEDED, or BLOCKED with precise review finding IDs.
Save the report outside the candidate.
```

# 7. Independently prove the candidate

Run this separately from review. It can run concurrently if your environment can truly freeze and isolate the same candidate.

```text
/prove <saved child contract>; candidate <saved implementation handoff>

Prove every child-contract requirement against this exact candidate.

Also enforce all applicable inherited P2PA-001 parent constraints.

For every requirement:
- observe the promised behavior at the agreed seam,
- use an independent oracle,
- retain the actual evidence,
- test material counterexamples and boundaries,
- distinguish a demonstrated violation from missing or inconclusive evidence.

Do not edit the candidate, tests, contract, or CI configuration.
Do not repair failures during proof.
Do not use green CI, a checked box, an implementation report, or a test name as
a substitute for evidence.

Account for every requirement ID.

Return PROVEN only if the entire child contract is established for this exact
candidate. Otherwise return NOT PROVEN and identify repairable requirement IDs
separately from agreement, authority, environment, or identity blockers.

Save the proof report outside the candidate.
```

Now you have the two independent questions answered:

```text
review → is the implementation sound and faithful?
proof  → is every promised outcome actually established?
```

# 8. If review finds a problem

If review gives, say, `F1` and `F3`, use `implement-contract`, not `repair-gaps`:

```text
/implement-contract <saved implementation handoff>; findings F1,F3

Correct only the supported review findings F1 and F3 against the current saved
contract.

First verify that the findings still apply to the captured candidate and current
code. Findings are evidence-backed inputs, not commands that must automatically
be obeyed.

Preserve the agreement and all valid existing evidence.
Do not fix unrelated concerns or broaden product scope.

If either finding actually requires changing a promised outcome, boundary,
exclusion, or consequential seam, stop the dependent work and return an amendment
handoff to plan-acceptance instead of changing the contract.

Capture the resulting exact candidate.
```

After that candidate changes, **run full review and full proof again**.

Do not just rereview F1 and F3.

# 9. If proof finds an implementation/evidence gap

Suppose proof returns `NOT PROVEN` for `R3` and `R7`, and explicitly identifies them as repairable.

Use:

```text
/repair-gaps <saved NOT PROVEN proof report>; candidate <saved candidate handoff>; requirements R3,R7

Repair only the named implementation/evidence gaps R3 and R7 from this matching
proof report.

Confirm before editing:
- exact contract identity still matches,
- exact candidate still matches the proof,
- R3 and R7 are genuinely repairable implementation/evidence gaps,
- no changed promise, authority problem, environment problem, or identity
  uncertainty is being disguised as a repair.

Make the smallest complete repair for those requirements.
Preserve all valid checks and existing contract requirements.

Run focused development checks, capture the changed candidate, and report the
repair honestly.

Do not claim acceptance.

Fresh full /prove and fresh /review-implementation are required afterward.
```

Again, once the candidate changes:

```text
new candidate
     ↓
full review
+
full proof
```

No old `REVIEWED` or `PROVEN` result carries forward automatically.

# 10. If implementation reveals that the contract itself must change

This is deliberately **not** a repair prompt.

First record/approve the proposed amendment. Then rerun planning:

```text
/plan-acceptance <child source reference>

Reconcile the following authorized amendment against the existing child contract.

Affected requirements: <IDs>

Previous agreement:
<exact previous promise/boundary/exclusion>

Authorized new agreement:
<exact approved replacement>

Authorization:
<durable approval/source reference>

Dependent implementation:
<what cannot proceed until this is reconciled>

Preserve unaffected requirement IDs.
Increment the contract revision only if this is a material semantic change.
Preserve the previous revision.

Return the complete revised contract.
Do not implement it.
```

Save and reread that revised contract, then resume implementation against the **new exact revision**.

All old proof about the changed agreement remains historical.

# 11. Complete each child before moving through dependencies

For each child, completion means:

```text
exact child contract
        +
exact child candidate
        +
current REVIEWED
        +
current PROVEN
```

It does **not** mean the parent P2PA-001 specification is proven.

This is especially important for A1/A2/B/C/D. A child being green simply establishes that child outcome and its contribution.

After the direct-path child B is working, and before implementing correction and recovery in C and D, consider a two-working-day TLA+ experiment. Model the decision reducer, parallel review/proof join, verification-epoch invalidation, and crash/restart effects. Turn any counterexample into a deterministic implementation test. If the experiment finds no protocol ambiguity or useful test case, continue without maintaining the model. This is not a release gate; R14-R23 and the host conformance tests remain required. Do not add Lean work to version 1.

# 12. Assemble the full integration candidate

Once all children are proven and their prerequisites are satisfied, create one exact integration candidate containing the complete v1 implementation in this repository. Record the exact proven Atomic host prerequisite and skill bundle used with it.

This should be a deliberate integration step, not “whatever happens to be on the branch.”

If assembling the children requires actual code changes, that integration work itself needs an assigned child/contract. If assembly is purely combining already-defined contributions, capture the resulting exact candidate and its provenance.

Seal the integration candidate even if its bytes happen to match the final integration child candidate. Child review and proof remain supporting evidence because they cover child contracts, not the complete parent contract.

# 13. Run decisive parent review and proof

These two fresh invocations establish whether **P2PA-001 itself** was delivered. They may run concurrently only if both receive isolated, immutable reconstructions of the same integration candidate and parent agreement.

First run the complete parent review:

```text
/review-implementation <integration candidate handoff> against <fixed integration comparison base>

Review this exact integration candidate against the complete saved P2PA-001
parent contract.

Treat child reviews as historical supporting evidence only. Perform complete
parent-contract coverage, including contract fidelity, scope, engineering
quality, authorization and role boundaries, deterministic protocol enforcement,
host compatibility, recovery behavior, and release conformance.

Bind the report to the exact parent agreement, source snapshot, integration
candidate, comparison base, verification epoch, Atomic host prerequisite, skill
bundle, and declared review environment.

Do not repair anything and do not perform acceptance proof.
Return REVIEWED only if the complete parent review is clean for this exact
candidate. Save and reread the report outside every candidate tree.
```

Run parent proof separately:

```text
/prove docs/acceptance-contracts/p2pa-001.md; candidate <integration candidate handoff>

Perform final parent proof for P2PA-001 against this one exact integration candidate.

Do not aggregate child PROVEN results into the parent verdict.
Historical child proofs are supporting references only.

Evaluate every parent requirement, including R1-R25, against the integrated
candidate and its actual execution environment.

In particular, exercise:
- durable versus memory admission,
- pinned-skill collision behavior,
- source/base validation,
- canonical contract save/readback,
- stable requirement handling,
- candidate snapshot round-trip,
- fresh review/proof isolation,
- verifier mutation denial,
- structured-result validation,
- proof completeness,
- review and proof repair routing,
- non-repairable gap handling,
- parallel join ordering,
- epoch/currentness invalidation,
- amendment authorization,
- forged-authority rejection,
- bounded convergence,
- checkpoint recovery,
- uncertain-effect recovery,
- interrupted mutation,
- cancellation and ownership fencing,
- final VERIFIED conjunction,
- Node/Bun host compatibility,
- portable artifact inspection.

Run the required real end-to-end fixture scenarios from the parent contract.
Use independently committed fixture expectations and actual observations.

Do not repair during this proof.
Do not treat child proof, CI, or workflow completion as parent acceptance.

Return PROVEN only if every parent requirement is established for the exact
integration candidate under the recorded Atomic host prerequisite and proof
environment.
```

The result is acceptable only when the parent review is `REVIEWED` and the parent proof is `PROVEN` for the same exact parent agreement, source snapshot, integration candidate, comparison base, and verification epoch. Both must record the pinned Atomic host prerequisite and skill bundle. Before treating the work as complete, reread both reports and confirm that all R1-R25 evidence remains retrievable, no writer or verifier is active, and no authority, amendment, source, identity, or policy decision is pending.

Record the final result as `VERIFIED` with `merge_readiness: NOT_ASSESSED`. CI, repository review, commit, push, merge, and deployment remain separate. If any completion condition fails, preserve the artifacts and follow the applicable correction, amendment, or blocker route instead of declaring success.

## The overall sequence

The process is therefore:

```text
P2PA-001 specification
        │
        ▼
plan-acceptance
        │
        ▼
parent contract
        │
        ▼
slice-contract
        │
        ├──── child A1 ─ plan → implement → review + prove
        │
        ├──── child A2 ─ plan → implement → review + prove
        │
        ├──── child B  ─ plan → implement → review + prove
        │
        ├──── child C  ─ plan → implement → review + prove
        │
        └──── child D  ─ plan → implement → review + prove
                              │
                              ▼
                          integration candidate
                              │
                        parent review + parent proof
                              │
                        completion gate
                          │
                            VERIFIED
```

The critical idea is that **we use Promise to Proof to implement the Atomic integration before the Atomic integration exists**. Initially, a human/enclosing agent manually invokes and carries these skills between stages. Once the project is finished, the product we have built is precisely the Atomic runtime that can automate this same sequence for future software work.

I would start with the first two prompts now: **parent `plan-acceptance`, then `slice-contract; draft only`**. Everything else should be driven by the contracts and decomposition those two produce.
