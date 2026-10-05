# Review ledger template

Copy this structure into local working notes for each PR. Fill orchestration
sections for full reviews; for delta-only operations mark unneeded fields N/A
with the operation scope. Link evidence once; other sections refer to its ID.
Keep the ledger local until the user chooses what to share.

## Review contract

```md
### Purpose

- Change:
- Success criteria:
- Ticket/spec and standards:
- What would invalidate the result:

### Operating model

- Product stage:
- Primary execution path:
- Inputs and expected scale:
- Concurrency:
- Target environments:
- Operator supervision:
- Detection and observability:
- Retry and manual recovery:
- Cost of failure or duplication:

### Review bar

- Merge blocker gates:
- Rollout gates:
- Pilot questions:
- Explicit non-goals or accepted debt:
- Accepted debt source and factual boundary:

### Assumptions and unknowns

-
```

## Frozen packet and ownership

```md
- Operation: Full / Fresh integration / Comparison / Pending edit / Publication
- PR:
- Exact base/head:
- Packet ID and sources: complete diff, PR, ticket/spec, standards, contract
- Explicit roster override and scope (or default Sol + Grok):
- Outcome owner (Sol discovery sealed before taking this role):
- Verification owner and execution permissions:
- Bootstrap owner/head/result:
- Independent code-review axes, if explicitly invoked:
```

## Discovery roster and seals

Record actual identity from harness metadata, not just a role name. Keep initial
artifacts private until all requested lanes seal. Link the sealed artifact and
coverage record rather than copying their prose.

```md
| Lane | Requested provider/model/effort | Actual provider/model/effort; identity evidence | Head/packet | Seal time/artifact | Blind / Unblinded / Contaminated; source/time | Completed / Incomplete / Blocked | Coverage/gaps |
|---|---|---|---|---|---|---|---|
| Sol |  |  |  |  |  |  |  |
| Grok |  |  |  |  |  |  |  |

- Escalation reason/scope, if any:
- Optional timeout and remaining obligations, if any:
```

## Coverage and head integration

```md
| Area / changed behavior / relevant caller / spec or standard | Lane coverage evidence | Gap and owner | Current-head relevance |
|---|---|---|---|
|  |  |  |  |

- Previous/new head:
- New integration coverage renewed:
- Evidence retained with relevance check:
- Evidence invalidated and renewed:
```

## Verification evidence

One owner authors and executes permitted offline smoke/reproductions and required
relevant gates. Discovery-only lanes skip execution. Record shared results once
per unchanged head; distinguish a passing gate from proof of a candidate.

```md
| Evidence ID | Gate / smoke / reproduction | Owner | Head | Command and prerequisites | Result / artifact | Candidate links | Required before publication? |
|---|---|---|---|---|---|---|---|
| V-01 |  |  |  |  | Pass / Fail / Blocked / Unrun |  |  |

- Required relevant gates and source:
- Execution restrictions or missing prerequisites:
- Scoped credentials/network/paid authorization, if needed:
```

## Lane attribution and usage

Count unique confirmed mechanisms once globally; retain corroboration attribution.
Candidate IDs support the counts, including valid deferred risks and invalid
claims. Keep contamination distinct from finding disposition.

```md
| Lane/source | Unique confirmed IDs/count | Corroborating IDs/count | Deferred IDs/count | Invalid IDs/count | Contaminated/unblinded status | Elapsed time | Output tokens | Noncached input tokens | Cached input tokens |
|---|---|---|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |  |  |  |

- Usage source: Measured / Estimated / Unknown
- Cost: Estimated / Actually billed / Unknown; source and amount:
- Bot threads: separate source IDs and dedup mapping:
```

## Candidate summary

```md
| ID | Finding | Factual status | Internal disposition | Existing-feedback relationship | Author severity | Author-facing home | Pending action |
|---|---|---|---|---|---|---|---|
| F-01 |  | Verified / Supported / Assumed / Unknown |  | Covered / Extension / New / Unsupported | Blocking / Non-blocking / None | Inline / Review body / Local only | Keep / Edit / Add / Remove |
```

## Candidate detail

Use one block per candidate:

```md
### F-01: Concise factual title

- Diff anchor:
- Originating/corroborating lane or bot source:
- Packet/head and current-head relevance:
- Factual claim:
- Evidence:
- Preconditions:
- Causal path:
- Reproduction/evidence ID and unexercised links:
- Reachability in declared operation:
- Exposure:
- Consequence:
- Contract gate affected:
- Detection and containment:
- Recovery and cost:
- Amplification risk:
- Assumptions or unknowns:
- Existing-feedback relationship:
- Disposition:
- Disposition rationale:
- Reconsideration trigger:
- Proposed validation:
- Author severity: Blocking / Non-blocking / None
- Author-facing home: Inline / Review body / Local only
- Pending action: Keep / Edit / Add / Remove
```

Keep invalid/rejected candidates compact: claim, disconfirming evidence, and
disposition suffice. No finding quota truncates the ledger.

## External-feedback delta

```md
| Source claim/thread | Candidate | Covered / Extension / New / Unsupported | Verification ID | Disposition | Pending action |
|---|---|---|---|---|---|
|  |  |  |  |  |  |
```

## Proposed feedback

Map every proposed comment back to a calibrated candidate.

```md
### F-01 — `path/to/file.py:42-45`

issue(blocking): <required outcome>

<Verified behavior, credible operational scenario, and consequence.>
<Focused regression or acceptance evidence.>
```

Use `question(blocking):` for a genuine unresolved merge contract and
`suggestion(non-blocking):` only with an explicit deferral boundary.

## Deferred register

```md
| ID | Disposition | Why it does not block now | Reconsider when | Evidence to collect |
|---|---|---|---|---|
| F-02 | Pilot evidence |  |  |  |
```

## Publication audit

```md
- Review state: Local only / Pending / Published
- Head revision:
- Final-complete / Incremental / Incomplete; remaining obligations:
- Every requested lane completed and identity verified:
- Seal/contamination records complete:
- Complete current-head coverage and gaps accounted for:
- Required relevant gates passed at current head:
- Candidate-specific verification complete or explicitly qualified:
- Attribution and external-feedback delta complete:
- Aggregate pull-request review decision before submission:
- Repository review convention checked:
- Current UI body and comments re-read after manual edits:
- Pending review body and comments read through review-scoped or GraphQL APIs:
- Pending review comment count:
- Every candidate has one author-facing home:
- Review body contains only unique global feedback or is empty:
- Every body matches the ledger:
- Every anchor is on the current diff:
- Blocking/non-blocking labels match calibrated dispositions:
- Every non-blocking comment states its deferral boundary:
- Explicit publication approval received:
- Submission event matches unresolved inline severity:
- Own-PR request-changes limitation and COMMENT/blocking-label handling:
- Submitted review state:
- Aggregate pull-request review decision after submission:
- Published review read back:
- Review URL:
- Every posted body/anchor/state read back; mismatches:
- Stable discussion links:
```
