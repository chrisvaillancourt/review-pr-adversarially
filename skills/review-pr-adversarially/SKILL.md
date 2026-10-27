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
- Treat any new head that materially changes behavior as a fresh integration
  baseline, not only as resolution of previous threads. Review other new heads
  as delta re-reviews, using the orchestration reference's materiality rules.

For **full reviews, batch reviews, fresh integration reviews, and delta
re-reviews**, read and follow
[review-orchestration.md](references/review-orchestration.md) before discovery.
The session that orchestrates the review launches the discovery lanes; this
skill's text cannot start them. The roster follows the review tier (light,
standard, or deep) in that reference, and a tier's full roster is a completed
pass at that tier. Comparisons and pending edits stay scoped to the supplied
feedback or edits unless changed behavior warrants a new full review. Explicit
roster overrides remain explicit; Kimi is not a default or automatic high-risk
lane.

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
- conditions that would invalidate the experiment or release;
- ticket/spec requirements and relevant repository standards;
- explicit non-goals and factual accepted debt;
- trust boundary: who writes each input, and which inputs are hostile. The
  team's own services and internal artifacts are not hostile by default. That
  default does not cover vendor, customer, or user-supplied data, even when an
  internal service relays it, so name the writer of each input. Not hostile does
  not mean well-formed: internal writers can still send stale or older-schema
  data; and
- round budget: after the second review round, keep the acceptance target fixed;
  a new blocker then needs a named source (see the risk-calibration reference)
  showing it is reachable in the intended path, or must be an unmet ticket or
  spec requirement. A late find in older code still counts when it has one.

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

- Pin the exact base/head and inspect the complete diff and current affected code
  and tests.
- Trace changed behavior across relevant library, service, persistence,
  deployment, permissions, and schema boundaries.
- Check authoritative contracts rather than relying on names or plausible API
  shapes.
- Assign head-pinned execution to the designated verification owner, within
  repository/runtime permissions, when a claim depends on a real integration.
- In blind discovery, defer evaluative prior feedback until sealing; follow the
  orchestration reference for necessary early intent context and unblinded lanes.

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

Check for reimplementation: does the diff rebuild something the framework, a
locked dependency, or the repository already provides? Check the version the
lockfile pins, not the latest documentation. When a domain skill for the
framework is installed, load it for this check. A reuse candidate names the
built-in, the version that provides it, and the code the author could drop.

Audit every new or changed test. Each must fail on a plausible break of the
behavior it names. Flag tests that cannot fail, duplicates of another test,
change-detector tests that pin today's values or structure instead of behavior,
and contract defaults that no test pins. Every claim about what a test does or
cannot do needs a mutation result: the named break applied to a scratch copy of
the head, the tests run, and the result (caught or survives) recorded. Reasoning
from framework semantics is not enough; until a mutation runs, the claim is
Assumed. Discovery lanes propose the mutation; the verification owner runs it
(see the orchestration reference).

Add candidates to the ledger before drafting comments. A possible code path is a
hypothesis, not yet a blocker. Do not discard a real concern merely because it is
unlikely; preserve it for calibration.

## 5. Calibrate operationally

Read [risk-calibration.md](references/risk-calibration.md) before assigning any
disposition. Evaluate factual validity separately from review priority.

Before assigning dispositions, read how the change actually runs: the deployment
or runtime configuration, from the repository or system that deploys it, at the
deployed revision (else its remote default branch), not a local checkout.
Record in the ledger's operating model which flags are enabled in which
environment and which settings each environment sets, with the source and
revision. A risk behind a flag no
environment enables, or behind a default every environment overrides, rarely
blocks merge; see the disabled-path examples in the calibration reference. When
the configuration cannot be read, mark the dependent links Unknown.

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

For full reviews, use the orchestration reference's sealed discovery and
verification handoff. Give the calibrator raw artifacts and the review contract,
not desired dispositions. Comparison-only work does not require a new ensemble.

## 6. Synthesize proportionate feedback

By default, propose author-facing comments only for merge blockers and concrete
rollout prerequisites. Keep pilot evidence and deferred hardening in the local
ledger unless the user asks to share them or they provide important context.
Test debt from the test audit (a test that cannot fail, a duplicate test) is the
exception: share it as a nitpick, because such a test gives false confidence to
every later change.

Drop a comment that asks nothing of the author, such as a note that something is
fine for this PR. Keep a risk that is possible but operationally unlikely at the
declared stage in the deferred register, not in a non-blocking comment, unless
the user asks to share it.

When most new findings in a round come from response commits, or repeat a class
from an earlier round, stop proposing per-finding patches. Ask for the invariant,
or a state or error table, in one design-level comment. For an equivalence or
redaction class that keeps growing, ask for a rule that refuses the whole class.

Before asking for a third fix round, check proportion: compare the diff with the
ticket or spec's scope and list the lines and branches each review round added.
If the PR grew mostly from review-response guards, stop requesting patches and
bring the approach, with any open blockers, to the user or approver.

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
or documented guidance before drafting. If no convention exists, default to
Conventional Comments, `label (decorations): subject`, with a space before the
decorations:

- **Merge blocker** → `issue (blocking):` or `question (blocking):`.
- **Rollout prerequisite** → `(blocking)` when the changed code can activate
  without the prerequisite, or when the current PR contract requires it before
  merge; otherwise `(non-blocking)` with the exact activation, deployment, or
  workload boundary that makes it required.
- **Pilot evidence** or **Deferred hardening** → local only by default. If shared,
  use `(non-blocking)` and state the reconsideration trigger.
- **Test debt** from the test audit (deferred hardening, shared by default) →
  `nitpick (non-blocking):`, citing the mutation that survives or the test it
  duplicates, with no reconsideration trigger.
- **Invalid or unsupported** → no author-facing comment.

Every `(non-blocking)` comment other than a nitpick states why merge remains
safe, what may be deferred, and the exact condition that makes follow-up
required.

When repository standards set a severity floor for listed cases, apply it to the
author-facing label, keep the calibrated disposition in the ledger, and tell the
user where they differ. Do not drop a case the repository requires you to
report, and do not treat a repository priority label, such as P1, as a merge
blocker by itself.

### Give each finding one author-facing home

- Give each code-local causal mechanism one inline thread.
- Extend a thread when new evidence concerns the same mechanism at the same
  boundary.
- Use a new inline thread when a response fix introduces a distinct mechanism at
  another persistence, recovery, deployment, or downstream boundary, even when
  both mechanisms violate the same broad invariant. Link the earlier thread when
  that context helps. When most new findings come from response commits or
  repeat an earlier class (see above), lead with the design-level comment and
  link these threads from it.
- Use the review body only for unique, author-actionable feedback that cannot be
  anchored to the diff.
- Leave the review body empty when every finding is inline.

The local report may contain the contract, ledger, dispositions, uncertainties,
and publication state. The GitHub review body contains only unique global
feedback—not workflow status, a review title, skill terminology, a severity
legend, publication metadata, or an inline-comment rollup. A design-level comment
may cite the threads its invariant covers; that is not a rollup.

### Draft comments with the repository convention

```md
issue (blocking): <required outcome>

<Verified behavior, credible operational scenario, and consequence.>
<Focused regression or acceptance evidence.>
```

```md
question (blocking): <specific unresolved contract>

<Why the answer determines whether the PR can merge.>
<Acceptable evidence or implementation outcomes.>
```

```md
suggestion (non-blocking): <deferred improvement>

<Verified weakness and why merge remains safe.>
This can be deferred until <boundary>; address it before <trigger>.
```

```md
nitpick (non-blocking): <test> <cannot catch the named break, or duplicates another>

<The mutation applied and the still-passing result, or the duplicated test.>
```

Use questions for genuine unknowns. Do not phrase an unverified prediction as an
asserted defect.

When a fix fits the anchored lines, give it as a GitHub `suggestion` block so the
author can apply it in one click. The block replaces exactly the anchored lines,
so anchor the comment to the lines it replaces and keep their indentation. When
the fix spans other lines or files, use a plain code block and say where it
goes. Section 7 runs every block before publication.

````md
issue (blocking): <required outcome>

<Verified behavior and consequence.>

```suggestion
<replacement for exactly the anchored lines>
```
````

## 7. Audit comments and anchors

Before editing or publishing a GitHub pending review, read
[github-pending-reviews.md](references/github-pending-reviews.md).

Before any pending-review edit or publication:

- read the current review body and comments, including manual UI edits;
- compare every comment with the calibrated ledger and any external-feedback
  delta;
- confirm every causal mechanism has one author-facing home;
- confirm severity labels follow the repository convention and every
  non-blocking comment other than a nitpick has a deferral boundary;
- anchor each inline comment to the smallest current-diff line or range that
  contains the causal behavior;
- apply every `suggestion` block exactly as written, over exactly its anchored
  lines, to a scratch copy of the head, then run the relevant tests, linter, and
  type checker; fix or drop any block that fails one of them;
- check every claim about third-party behavior in that dependency's source at
  the locked version, or in its documentation when the source is unavailable;
- remove stale wording, exaggerated consequences, and comments whose
  disposition changed;
- keep the review body empty unless unique global feedback requires it; and
- report the file and line range for every proposed edit.

Pending-review authorization ends after mutation and readback. Publication still
requires an explicit user command.

### Run the final gate before asking to publish

Run the gate on the comment set meant for publication, so the user approves the
validated text:

1. **Clarity pass on prose only.** Tighten the prose, for example with the
   `unslop` and `writing-clearly-and-concisely` skills when installed. Leave
   fenced blocks, identifiers, and anchors alone, and check that every fenced
   block is byte-identical before and after the pass.
2. **Cross-model check of the frozen set.** Freeze the bodies, anchors, and
   head. Send the frozen set and its evidence to one validator in the
   orchestration reference's data, fact, and deliverable checks role; comments
   going to a colleague are a deliverable. Ask for wrong or unsupported claims,
   overstatement, and anchors that do not hold, and verify each finding before
   acting on it. Rerun the section 7 block checks on any fenced block that
   changes.
3. **Delta round for later changes.** Send any comment added or materially
   reworded after the check to the same validator session, resumed, as a delta
   round. Do not publish text the validator has not seen. When the validator is
   unavailable, report the blocker as for an unavailable lane; do not
   substitute silently.

## 8. Publish and read back only when authorized

After explicit authorization and a passed final gate (section 7), follow the
publication conditions in
[review-orchestration.md](references/review-orchestration.md#4-integrate-complete-results-at-the-current-head)
for either final-complete publication or an early incremental verified blocker.
That reference governs completion gates and the incremental exception; an
incremental review is never final-complete. Choose the submitted event from inline
severities:

- any unresolved `(blocking)` comment → request changes;
- only `(non-blocking)` comments → comment; and
- approve only when the user explicitly requests approval or the surrounding
  workflow already establishes that decision.

When GitHub prohibits requesting changes on the reviewer's own PR, use `COMMENT`
with substantive `(blocking)` feedback and report the platform limitation. Never
submit an empty status-only formal review; status belongs in the local report.

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
register, uncertainties, publication state, review tier, and lane/verification
completeness.

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
