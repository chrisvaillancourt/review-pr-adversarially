# Review ledger template

Copy this structure into working notes for each review. Keep the ledger local
until the user chooses what to share.

## Review contract

```md
### Purpose

- Change:
- Success criteria:
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

### Assumptions and unknowns

-
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
- Factual claim:
- Evidence:
- Preconditions:
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
- Submitted review state:
- Aggregate pull-request review decision after submission:
- Published review read back:
- Review URL:
```
