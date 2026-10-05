# Blind review orchestration

Use this sequence for a full review, batch of PRs, or fresh integration review.
For a comparison or pending-comment edit, verify the supplied delta and affected
behavior; start a new ensemble only when changed behavior requires renewed full
coverage. Each PR has its own packet, seals, ledger, and verification state.

## 1. Freeze the packet and resolve the roster

Pin the exact base and head commits, complete diff, PR description, ticket/spec,
relevant repository instructions and standards, and factual operating contract.
List accepted debt as facts, not recommendations about finding dispositions.
Give every discovery lane the same complete packet and access to relevant
callers, tests, dependencies, and source contracts. File partitions or exclusive
lenses cannot substitute for full behavioral coverage.

The default discovery roster is **clean-context GPT-6.1 Sol plus Grok**, launched
concurrently. Resolve these role names to actual available provider/model IDs
and effort settings before launch; record the requested and actual identities
and the harness metadata that verifies them. A role label or reviewer nickname
is not model-identity evidence. Prefer a specialized Grok reviewer when available;
otherwise explicitly select a Grok model in a general read-only reviewer. No
particular harness tool name is required. If either requested model is unavailable
or its identity cannot be verified, report the concrete blocker; do not substitute
silently or record a completed pass. Honor explicit user roster overrides and
record their scope. Kimi is not a default lane, including for high-risk reviews.

Designate one outcome owner and one verification owner early. Primary Sol may
own the outcome only after sealing its own discovery. Scheduling and outcome
ownership do not add another full-code discovery pass. A separate clean Sol
escalation is optional only for a high-risk or disputed causal chain or a coverage
gap: record the specific reason and scope, then apply the same identity and seal
rules. Do not escalate unconditionally.

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
that a defect exists or does not exist.

Return compact raw candidates: preconditions, causal path, consequence, exact
evidence/anchor, proposed reproduction, and coverage gaps. Include all supported
candidates; no fixed quota may truncate actual findings. Record rejected claims
briefly with their disconfirming evidence, not narrative rejection essays.

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

A new head is a fresh integration baseline. Refresh the packet, selectively
invalidate candidate and gate evidence affected by the changes, and renew full
coverage of the resulting integration, not just prior threads. Preserve still
valid evidence with its original head and a recorded current-head relevance
check; gates for an old head do not count as current-head passes.

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
