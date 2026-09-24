---
name: review-pr-adversarially
description: Perform evidence-backed adversarial reviews of pull requests and diffs while calibrating technically valid findings to the declared MVP, pilot, rollout, or production bar. Use when asked to deeply or adversarially review code, re-review a large response commit, compare an independent review or audit with existing feedback, distinguish operationally relevant blockers from unlikely hardening concerns, prepare or audit pending inline comments, verify diff anchors, or publish an approved review. Separate discovery from operational calibration, preserve deferred risks with reconsideration triggers, and require explicit approval before publishing feedback.
---

# Review PR Adversarially

Find broadly, judge narrowly, and publish deliberately. Treat adversarial findings
as candidates until their operational relevance is established against an explicit
review contract.

## Preserve the review boundary

- Read repository instructions before reviewing code.
- Treat request verbs as narrow stage authorization:
  - `review`, `compare`, `draft`, or `prepare` means local analysis only;
  - `update` or `edit` a pending review authorizes pending mutations and readback,
    but the review remains pending;
  - `post`, `publish`, or `submit` authorizes publication after a fresh audit.
- Treat a materially changed review-response commit as a fresh integration
  baseline, not only as resolution of previous threads.

## 1. Establish the review contract

Gather the contract from the user, ticket, PR description, design documents, and
target-environment evidence. State assumptions when the sources do not settle a
question. Ask only when an unknown would materially change the review bar.

Capture:

- product stage: supervised MVP, pilot, rollout, or production;
- purpose and success criteria;
- expected execution path, inputs, scale, and concurrency;
- target environments and external contracts;
- operator supervision and observability;
- retry, rollback, and manual-recovery options;
- cost of failed, repeated, duplicated, or corrupted work;
- conditions that would invalidate the experiment or release; and
- explicit non-goals and accepted debt.

Define the blocker gates before deep discovery. For a supervised pre-production
MVP, start with these gates unless the evidence requires a different bar:

- the core flow cannot run;
- validation data or conclusions can be corrupted;
- failure can appear to be success;
- materially expensive work can become stuck or duplicated; or
- a required target environment cannot exercise the flow.

Use the contract template in
[review-ledger-template.md](references/review-ledger-template.md).

## 2. Build the evidence baseline

- Inspect the complete diff and the current versions of affected code and tests.
- Trace changed behavior across relevant library, service, persistence,
  deployment, permissions, and schema boundaries.
- Check authoritative contracts rather than relying on names or plausible API
  shapes.
- Exercise the narrowest real integration path when a claim depends on an
  installed library or cross-system configuration.
- Reconstruct prior comments only to understand intent; independently inspect
  all materially changed behavior.

Record exact evidence and diff anchors. Distinguish verified behavior from
inference, prediction, and unresolved assumptions.

## 3. Compare external feedback as a delta when supplied

When another review, audit, or agent report is provided, split it into atomic
claims before mapping it. A mixed report may contain findings, corroboration,
rejected hypotheses, and process notes.

1. Re-read the current PR head and every current review comment.
2. Map every external claim to exactly one relationship:
   - already covered;
   - extension of an existing root cause;
   - genuinely new; or
   - invalid or unsupported.
3. Independently verify extensions and new findings. External severity and
   recommendations are inputs to investigate, not evidence.
4. Assign each author-facing action using the
   [one-home rules](#give-each-finding-one-author-facing-home).
5. Keep duplicates, invalid findings, and calibrated local-only risks out of the
   author-facing review unless the user explicitly asks to share them.

The delta is complete when every external claim has a relationship, evidence
status, calibrated disposition, and pending-feedback action.

## 4. Discover adversarially

Search aggressively for correctness, state-transition, recovery, error-scope,
identity, persistence, concurrency, deployment, and validation failures. For each
candidate, describe:

1. the specific behavior;
2. the preconditions required to trigger it;
3. the path from those preconditions through the changed code;
4. the resulting consequence; and
5. the evidence supporting each link.

Challenge identity and cardinality assumptions against authoritative source
models. Trace identifiers, aliases, versions, revisions, currentness,
supersession, and migration from the writer through persistence, selection, and
conflict handling. A tested application invariant may still reject valid
upstream data. Do not infer that the highest version is authoritative without a
source contract.

Add candidates to the ledger before drafting comments. A possible code path is a
hypothesis, not yet a blocker. Do not discard a real concern merely because it is
unlikely; preserve it for calibration.

## 5. Calibrate operationally

Read [risk-calibration.md](references/risk-calibration.md) before assigning any
disposition. Evaluate factual validity separately from review priority.

For each candidate:

- prove or qualify its preconditions;
- determine whether the intended workflow will exercise them;
- assess consequence against the declared blocker gates;
- account for detection, containment, recovery, and cost;
- identify whether the risk belongs to merge, rollout, pilot learning, or later
  scale; and
- record the evidence that would change the disposition.

Assign exactly one disposition:

- **Merge blocker**
- **Rollout prerequisite**
- **Pilot evidence**
- **Deferred hardening**
- **Invalid or unsupported**

Do not use a numeric score as a substitute for the operational story. Low
probability alone does not make a risk non-blocking when the trigger is credible
and the consequence is irreversible, silent, unsafe, or experiment-invalidating.

When independent agents are available and the review is large enough to benefit,
separate discovery and calibration. Give the calibrator the raw artifacts, review
contract, and candidate ledger—not the desired dispositions.

## 6. Synthesize proportionate feedback

By default, propose author-facing comments only for merge blockers and concrete
rollout prerequisites. Keep pilot evidence and deferred hardening in the local
ledger unless the user asks to share them or they provide important context.

Before drafting a comment, verify:

- the factual claim is supported;
- the operational scenario is credible for the declared stage;
- the consequence meets the stated disposition;
- existing detection and recovery are acknowledged;
- the request describes the required outcome rather than an unnecessarily narrow
  mechanism; and
- the requested regression is focused and proportional.

Run author-visible feedback through a plain-language pass aligned with ISO
24495-1:2023: put the requested outcome first, use short sentences, name the
actor and trigger, define only necessary terms, and preserve exact technical
identifiers.

### Translate internal disposition to author-facing severity

Internal dispositions are analysis vocabulary. Author-facing comments use the
repository's established convention; inspect recent repository review comments
or documented guidance before drafting. If no convention exists, default to:

- **Merge blocker** → `issue(blocking):` or `question(blocking):`.
- **Rollout prerequisite** → `(blocking)` when the changed code can activate
  without the prerequisite, or when the current PR contract requires it before
  merge; otherwise `(non-blocking)` with the exact activation, deployment, or
  workload boundary that makes it required.
- **Pilot evidence** or **Deferred hardening** → local only by default. If shared,
  use `(non-blocking)` and state the reconsideration trigger.
- **Invalid or unsupported** → no author-facing comment.

Every `(non-blocking)` comment states why merge remains safe, what may be
deferred, and the exact condition that makes follow-up required.

### Give each finding one author-facing home

- Give each code-local causal mechanism one inline thread.
- Extend a thread when new evidence concerns the same mechanism at the same
  boundary.
- Use a new inline thread when a response fix introduces a distinct mechanism at
  another persistence, recovery, deployment, or downstream boundary, even when
  both mechanisms violate the same broad invariant. Link the earlier thread when
  that context helps.
- Use the review body only for unique, author-actionable feedback that cannot be
  anchored to the diff.
- Leave the review body empty when every finding is inline.

The local report may contain the contract, ledger, dispositions, uncertainties,
and publication state. The GitHub review body contains only unique global
feedback—not workflow status, a review title, skill terminology, a severity
legend, publication metadata, or an inline-comment rollup.

### Draft comments with the repository convention

```md
issue(blocking): <required outcome>

<Verified behavior, credible operational scenario, and consequence.>
<Focused regression or acceptance evidence.>
```

```md
question(blocking): <specific unresolved contract>

<Why the answer determines whether the PR can merge.>
<Acceptable evidence or implementation outcomes.>
```

```md
suggestion(non-blocking): <deferred improvement>

<Verified weakness and why merge remains safe.>
This can be deferred until <boundary>; address it before <trigger>.
```

Use questions for genuine unknowns. Do not phrase an unverified prediction as an
asserted defect.

## 7. Audit comments and anchors

Before editing or publishing a GitHub pending review, read
[github-pending-reviews.md](references/github-pending-reviews.md).

Before any pending-review edit or publication:

- read the current review body and comments, including manual UI edits;
- compare every comment with the calibrated ledger and any external-feedback
  delta;
- confirm every causal mechanism has one author-facing home;
- confirm severity labels follow the repository convention and every
  non-blocking comment has a deferral boundary;
- anchor each inline comment to the smallest current-diff line or range that
  contains the causal behavior;
- remove stale wording, exaggerated consequences, and comments whose
  disposition changed;
- keep the review body empty unless unique global feedback requires it; and
- report the file and line range for every proposed edit.

Pending-review authorization ends after mutation and readback. Publication still
requires an explicit user command.

## 8. Publish and read back only when authorized

After explicit authorization, audit the current head and comments, then choose
the submitted review event from the inline severities:

- any unresolved `(blocking)` comment → request changes;
- only `(non-blocking)` comments → comment; and
- approve only when the user explicitly requests approval or the surrounding
  workflow already establishes that decision.

Read the aggregate pull-request review decision before submission. Submitting
`COMMENT` does not clear an earlier `CHANGES_REQUESTED` decision; report both the
new review state and the resulting aggregate decision.

An empty review body is valid when substantive feedback is inline. After
submission, retrieve the posted review and confirm:

- review state and body;
- aggregate pull-request review decision;
- head revision;
- every comment body, file, and line range; and
- stable review and discussion links.

Report any mismatch immediately. Do not silently repair posted feedback unless
the user authorizes the edit.

## Output by operation

### Read-only or full review

Present the review contract, candidate ledger, proposed feedback, deferred
register, uncertainties, and publication state.

### Independent-review comparison

Present the delta: already covered items, extensions, genuinely new findings,
rejected claims, proposed pending-review actions, and publication state.

### Pending-review edit

Report changed comment IDs and anchors, review head, comment count, and
confirmation that the review remains pending.

### Publication

Report the submitted state, resulting aggregate review decision, head revision,
review URL, comment count, outdated count, and any readback mismatch.

Use [calibration-examples.md](references/calibration-examples.md) when a finding's
placement remains ambiguous.
