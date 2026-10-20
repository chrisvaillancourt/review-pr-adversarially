# Risk calibration

Use this rubric after adversarial discovery and before drafting author-facing
feedback. Its purpose is not to make real defects disappear. It determines when
the team needs to act on them.

## Separate the dimensions

Assess each dimension explicitly:

- **Factual validity:** Does the implementation actually behave as claimed?
- **Preconditions:** What inputs, state, timing, configuration, or prior failures
  are required?
- **Reachability:** Can those preconditions occur in the intended environment?
- **Exposure:** Will the declared workflow exercise them, and roughly how often?
- **Consequence:** What becomes wrong, unavailable, misleading, unsafe, or
  expensive?
- **Detectability:** Will automation or the supervising operator recognize it?
- **Recoverability:** Can the run or data be repaired safely, cheaply, and within
  the pilot window?
- **Amplification:** Can retries, concurrency, persistence, or paid side effects
  multiply the damage?
- **Evidence quality:** Which links are verified, supported, assumed, or unknown?

Do not collapse these dimensions into the phrase “edge case.”

## Write the operational story

A review recommendation requires a complete chain:

```text
credible preconditions
  -> reachable changed behavior
  -> consequence under the declared workflow
  -> effect on the review contract
```

Mark each link:

- **Verified:** Directly demonstrated by code, test, executable reproduction, or
  authoritative contract.
- **Supported:** Strongly implied by available evidence but not directly
  exercised.
- **Assumed:** Plausible but not established for the intended operation.
- **Unknown:** Required information is missing.

If a required link is assumed or unknown, investigate it or frame it as a
question. Do not present the full chain as established fact.

## Name the source of each input

A reachability claim names its source: a real call path, an enforced schema, a
supported contract, or observed upstream data that produces the triggering
input. Tests, fixtures, and example docs alone do not show that the input
arrives; a test can verify the behavior once it does. Without a named source,
the reachability link is Assumed. Code the reviewer cannot read is an evidence
gap, not proof either way: when the would-be source is such code, mark the link
Unknown and investigate it or ask. Judge each input on its own: one member of a
class can be reachable while another is not. An input the ticket or spec
promises to handle is a requirement: judge it against the spec, and lack of
observed traffic does not waive it.

Disposition follows the whole operational story above, reachability included;
record evidence quality separately. Never set a severity floor for a fixed list
of input cases, in a disposition or in a review rule you propose. Discovery
still reports candidates whose source it could not find; this rule applies
during calibration.

## Choose the disposition

### Merge blocker

Use when the behavior is credibly reachable in the intended path and violates a
declared merge gate. For a supervised pre-production MVP, typical gates are:

- the primary flow cannot run;
- validation data or conclusions can be corrupted;
- failure can appear to be success;
- materially expensive work can become stuck or duplicated; or
- the required MVP environment cannot exercise the flow.

Detection and manual recovery may reduce severity, but they do not rescue a flow
that cannot answer the MVP's core question.

### Rollout prerequisite

Use when merge is safe but a known environment, permission, migration,
configuration, runbook, or safety control must exist before enabling a particular
deployment or workload. State the rollout boundary precisely.

### Pilot evidence

Use when the concern is plausible and measurable but its operational importance
depends on cost, frequency, duration, scale, or recovery evidence the pilot is
intended to collect. Name the metric, observation window, and decision trigger.

### Deferred hardening

Use for a real weakness that does not threaten the current contract because its
preconditions are outside the intended workflow, its consequence is contained
and recoverable, or it depends on later scale or concurrency. Always record a
specific reconsideration trigger.

### Invalid or unsupported

Use when the claimed behavior is factually wrong, an authoritative contract
contradicts it, required preconditions cannot occur, or evidence is too weak even
to preserve it as a current risk. Record the reason so the same claim is not
recreated without new evidence.

## Test “factually correct but operationally unlikely”

Ask these questions in order:

1. Which exact preconditions make the behavior possible?
2. What evidence shows those conditions exist in the target operation?
3. Is the path part of the primary flow, an expected recovery flow, or only a
   hypothetical future mode?
4. If it happens once, does it invalidate the experiment, create a false green,
   corrupt durable state, or multiply expensive work?
5. Would supervision detect it before the result is used?
6. Can the team recover within the pilot's time and cost budget?
7. What pilot evidence would justify prevention work?

Common reasons to avoid blocking:

- the required scale or concurrency is explicitly outside the current stage;
- several independent rare failures must accumulate before any consequence;
- the state is bounded, visible, and cheaply recoverable;
- the issue affects optional or unused behavior;
- prevention would build speculative machinery that the pilot is intended to
  inform; or
- the only effect is a loud, fail-closed refusal of an input with no named
  source (see "Name the source of each input"): record it as deferred hardening
  by default, to reconsider when a source appears. This does not cover a miss
  that is silent or reads as success, or an input the ticket or spec promises to
  handle; weigh those under the reasons to block below or against the spec.

Common reasons to block despite low frequency:

- the happy path or required recovery path reaches it;
- the failure is silent or produces a false success;
- it irreversibly corrupts validation data;
- one occurrence can trigger unbounded or materially expensive duplication;
- operators cannot detect it before acting on the result; or
- the consequence is unsafe or violates a non-negotiable external contract.

## Match the stage

### Supervised pre-production MVP

Optimize for learning whether the core workflow works. Accept visible,
recoverable friction when it does not corrupt the experiment. Prefer pilot
instrumentation over speculative robustness.

### Limited rollout

Require the intended environment, permissions, migration path, monitoring,
bounded retries, and an actionable recovery path. Reassess risks exposed by real
traffic and reduced supervision.

### Production

Account for unattended operation, sustained scale, concurrency, tenant or data
isolation, irreversible changes, security boundaries, supportability, and formal
reliability expectations.

Do not infer the stage from branch names or the word “MVP” in isolation. Use the
declared operating model.

## Detect overstatement

Rework a finding when its wording:

- jumps from “can” to “will” without exposure evidence;
- calls a bounded retry or manual recovery impossible;
- assumes production scale for a supervised pilot;
- ignores an existing detection or containment mechanism;
- treats a library possibility as an application-reachable path;
- converts a rollout dependency into a code merge blocker without explanation;
- prescribes a mechanism before stating the invariant; or
- asks for exhaustive proof when a focused regression or pilot metric would
  protect the contract.
