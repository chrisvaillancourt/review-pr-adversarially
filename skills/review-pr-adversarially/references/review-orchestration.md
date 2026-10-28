# Blind review orchestration

Use this sequence for a full review, batch of PRs, fresh integration review, or
delta re-review of a new head (section 4). For a comparison or pending-comment
edit, verify the supplied feedback or edits and the affected behavior; start a
new ensemble only when changed behavior requires renewed full coverage. Each PR
has its own packet, seals, ledger, and verification state.

## 1. Freeze the packet and resolve the roster

Pin the exact base and head commits, complete diff, PR description, ticket/spec,
relevant repository instructions and standards, and factual operating contract.
List accepted debt as facts, not recommendations about finding dispositions.
Give every discovery lane the same complete packet and access to relevant
callers, tests, dependencies, and source contracts. File partitions or exclusive
lenses cannot substitute for full behavioral coverage.

The session that orchestrates the review launches the discovery lanes before
discovery starts; this skill's text cannot launch them. An agent assigned
orchestration that cannot launch every lane the tier requires returns the frozen
packet and the missing lanes to its caller before launching any lane, when the
caller can launch them, so one orchestrator owns every seal; otherwise it
launches the lanes it can and reports the review incomplete at that tier.
An agent assigned one discovery lane does discovery only (section 2): it does not
resolve the roster, launch lanes, or return the packet, and it reports lane
completion, not whole-review completion. Say so in each lane's prompt. Every
lane runs in a fresh context that did not write the change; at the light tier,
an orchestrating session that is itself such a context may be the one lane.

Take the review tier from the caller or user, or choose it from the review
contract, and record it before launch. When the contract calls for a higher tier
than the caller set, use it unless the user explicitly overrides it, and record
why. The tier sets the roster; the product stage sets the blocker bar. Never
lower the tier to fit the lanes that happen to be available.

Take the roster from the user's instructions when they define review lanes:
the models, efforts, how to launch each from the current harness, the final-gate
validator, and any fallback rule. Otherwise use these defaults:

| Tier | Typical change | Default discovery lanes |
|---|---|---|
| Light | Docs, tests, internal tooling, small reversible changes | One fresh-context lane, any model |
| Standard | Other behavior changes (the default) | A fresh context of the orchestrator's model + one lane from another model family |
| Deep | Auth or security boundaries, money, irreversible data, external contracts | The standard lanes + one lane from a third model |

A model family is a vendor's model line, such as Claude or GPT. Every lane runs
in a fresh context that did not write the change and gets the complete packet.
Lanes may run concurrently or one after another; either way, no lane sees
another's work before sealing (section 2). A tier's full roster, completed, is a
completed pass at that tier. A pass at a lower tier than the recorded tier is
incomplete for this review. An optional lane, once launched, is a requested lane.
Resolve the final-gate validator (the core skill's section 7) along with the
lanes, before discovery starts, so an unavailable validator is found early.

Resolve each lane to an actual available provider/model ID and effort setting
before launch; record the requested and actual identities and the harness
metadata that verifies them. A role label or reviewer nickname is not
model-identity evidence. No particular harness tool name is required. Record the
model families that actually ran.

If a lane the tier requires is unavailable or its identity cannot be verified,
apply the fallback rule from the user's instructions, recording the substitution
and its reason. With no such rule, report the concrete blocker. Never substitute
silently. Without user rules, one fallback applies: at the standard tier, when no
model of a second family can be launched, a second fresh context of the
orchestrator's model may stand in; record the pass as single-family, with the
reason, and report it complete with that limitation. A deep pass never completes
with only one family; an explicit user waiver makes it a recorded custom review,
not a completed deep pass. Honor explicit user roster overrides and record their
scope.

Designate one outcome owner and one verification owner early. A discovery lane
may own the outcome only after sealing its own discovery. Scheduling and outcome
ownership do not add another full-code discovery pass. A separate clean
escalation lane is optional only for a high-risk or disputed causal chain or a
coverage gap: record the specific reason and scope, then apply the same identity
and seal rules. Do not escalate unconditionally.

## 2. Discover blind and seal privately

Each lane independently traces all changed behavior, relevant callers, and
contract/standards obligations. Discovery-only lanes skip tests, gates, and
formatters. Keep their initial candidate artifacts private until each requested
lane has sealed its initial findings and coverage gaps. Readiness notices contain
only lane identity, pinned head, and ready/sealed status, never findings.

During discovery, lanes do not read peer history, artifacts, ledgers, results, or
cross-PR broadcasts. Delay evaluative prior reviews and bot comments until seals
are recorded. If intent-dependent author feedback is needed earlier, disclose
only the necessary context and mark that lane unblinded, with source and time.
Record accidental contamination too; never describe such a lane as blind. When
clean independence is required, obtain a later clean pass rather than relabeling
the exposed lane. User feedback remains authoritative about intent, not evidence
that a defect exists or does not exist. Delta re-review lanes are unblinded by
design: section 4 gives them the fixed findings, the deferred register, and the
rejected hypotheses, so record all three as the unblinding source.

Return compact raw candidates: preconditions, causal path, consequence, exact
evidence/anchor, proposed reproduction, and coverage gaps. Include all supported
candidates, including those whose input source is not yet found; no fixed quota
may truncate actual findings. Record rejected claims briefly with their
disconfirming evidence, not narrative rejection essays.

## 3. Verify once, then calibrate

Bootstrap the repository once per owned PR/head. The designated verification
owner reads repository/runtime restrictions, discovers required relevant gates,
and authors **and executes** the narrow offline smoke or reproduction needed for
candidate claims. The calibrator may fill this role only when higher-level
permissions allow execution. Discovery lanes remain read-only. Do not bypass
execution restrictions: record blocked verification and the exact prerequisite.
Credentials, network access, and paid operations need scoped authorization;
offline verification does not imply authorization for those operations.

Run required relevant gates once per unchanged head, early enough to inform the
outcome. Apply the publication conditions in section 4 to final-complete and
incremental feedback. Share the head-pinned results rather than repeating
bootstrap or gates per lane. Record command, owner, head, result, and evidence.
A passing suite alone does not prove a causal claim; use the smallest relevant
smoke/reproduction and qualify unexercised links.

The verification owner also runs the test audit's mutations. Apply each break to
a scratch copy of the head (for example, a `git archive` export) rather than a
worktree other lanes are reading, run it with the review's environment, and
first confirm that imports resolve to the copy. Make exact-match edits only after
checking that the anchor occurs once, so a mutation cannot also change an
identical line elsewhere, such as inside a test's embedded script. Record each
mutation, the tests run, and whether the break was caught or survived.

After sealing, the outcome owner receives raw candidates and evidence without
supplied desired dispositions and applies the skill's risk-calibration reference.
Keep bot threads as a separate source, map them to causal mechanisms, and dedup
against lane findings. Preserve attribution when merging corroborating evidence.
Do not serialize full skill chains per reviewer. If `code-review` is explicitly
invoked, retain its documented review axes as distinct obligations in the packet
and coverage record rather than pretending adversarial discovery replaces them.

## 4. Integrate complete results at the current head

Completion waits for **every requested lane and required verification**. Optional
timeouts are recorded as incomplete, not completed or clean passes; never drop a
required model to declare success. Report incomplete reviews as such. Keep a
coverage map for the complete diff, changed behavior, relevant callers, spec,
and standards; close or explicitly expose each gap.

The outcome owner classifies each new head and records the reason. A new head
that materially changes behavior (a new state, path, identity, or error scope)
is a fresh integration baseline. A merge or rebase of the base is material when
the base changed code the PR touches or calls. Refresh the packet, selectively
invalidate candidate and gate evidence affected by the changes, and renew full
coverage of the resulting integration, not just prior threads. Preserve still
valid evidence with its original head and a recorded current-head relevance
check. Fresh-baseline lanes stay blind; the calibrator, not the lanes, checks
their candidates against the deferred register.

Review any other new head as a delta: the fixed findings and the commits since
the last reviewed head (for a rebase, the range-diff), carrying earlier coverage
of unchanged code with its original head. Delta re-review lanes get the fixed
findings to verify against the new commits, the deferred register as accepted
contract facts, and the rejected hypotheses with the evidence that rejected
each. They raise a deferred item again only when its reconsideration trigger has
fired, which includes finding that the trigger's condition already holds. They
raise a rejected hypothesis again only when the new commits change the evidence
that rejected it, or the lane brings evidence the rejection did not weigh that
overturns it: a named source, or a ticket or spec promise, for a reachability
rejection (see "Name the source of each input" in the risk-calibration
reference); otherwise verified behavior, an authoritative contract, or an unmet
ticket or spec requirement. Restating the rejected claim is not new evidence.
When unsure whether a change is material, treat the head as a fresh baseline. On
every new head, gates for an old head do not count as current-head passes.

### Publication conditions

**Final-complete publication** requires every requested lane to complete,
current-head coverage to be accounted for, and all required relevant gates to
pass. Failed, unsafe-to-run, or unavailable required gates block final-complete
publication; record the exact prerequisite locally.

**Early incremental publication** is permitted after explicit publication
authorization only for a calibrated, verified blocker. It applies after the
initial seal barrier in section 2, but before remaining lane completion,
current-head coverage, or final verification is complete. Keep initial candidates
private until every requested discovery lane has sealed its initial findings and
coverage gaps; early feedback never justifies exposing an unsealed candidate.
Before publishing the blocker:

- complete the verification and pass the required gates relevant to that blocker;
  failed, unsafe-to-run, or unavailable relevant gates block its publication—do
  not replace verification with a speculative warning to the author;
- audit the current head, the causal evidence, and the current diff anchor;
- audit existing feedback, including current comments and pending/manual edits,
  and enforce one author-facing home per causal mechanism; and
- record the incremental status and all remaining lanes, coverage gaps, and checks
  in the local report, never in the formal review body or as a final-complete
  result.

Keep incremental author feedback out of still-blind peer reviewers' packets,
history, and feedback reads, including later clean passes or renewed discovery.
If a lane is exposed, record the source and time as contamination/unblinding and
apply section 2's clean-pass requirement; never claim it remained blind.

Both publication paths require calibrated evidence, audited existing feedback,
one author-facing home per mechanism, and fresh current-head anchors. Incremental
publication does not discharge any remaining full-review obligations.

## 5. Publish deliberately and record attribution

Follow the core skill's pending permissions, one-home rules, and publication
readback. If GitHub prohibits `REQUEST_CHANGES` on the reviewer's own PR, submit
substantive feedback as `COMMENT` with explicit `(blocking)` labels; report that
platform limitation and the aggregate decision separately. Do not create an
empty status-only formal review. Workflow/completion status belongs in the local
report, not the author-facing review body.

Record per lane: unique confirmed mechanisms, corroborating mechanisms,
deferred candidates, invalid candidates, contamination/unblinding, coverage,
seal, and completion. Count merged mechanisms once globally; attribution is not
an excuse for duplicate comments. Track elapsed time, output tokens, noncached
input tokens, and cached input tokens separately when exposed. Distinguish
measured usage, estimates, and actual billed cost; unknown values stay unknown.
Use the review-ledger template as the portable record, linking each fact once
rather than repeating discovery prose in every outcome artifact.
