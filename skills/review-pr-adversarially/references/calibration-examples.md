# Calibration examples

These examples illustrate placement, not universal severity. Re-evaluate each one
against the actual review contract.

Internal dispositions below remain local analysis vocabulary. Translate them to
the repository's author-facing severity convention only during synthesis.

## Primary library path is incompatible

The changed configuration uses an option shape rejected by the installed library,
so the primary read fails before validation begins.

- Factual status: verified with the installed library.
- MVP relevance: exercised on every primary run.
- Disposition: **Merge blocker** because the core flow cannot run.
- Feedback: require a compatible shape and a focused real-library regression.

## Failed work is persisted as success

A terminal external-service failure produces a completed manifest row, so later
runs skip the document and the aggregate result appears successful.

- Factual status: verified through the state transition.
- MVP relevance: the pilot may use the manifest to judge completion.
- Disposition: **Merge blocker** because failure can appear to be success and
  corrupt validation conclusions.
- Low frequency does not neutralize the false-green consequence.

## Target permission is absent

The new implementation requires an object-version read permission that is absent
from the production role, while the supervised development environment can run
with broader credentials.

- Factual status: verified against deployment policy.
- MVP relevance: depends on the declared target environment.
- Disposition: **Rollout prerequisite** if the MVP need not run under that role;
  **Merge blocker** if that environment is required for the current experiment.
- Feedback must name the environment boundary rather than claiming the code fails
  everywhere.

## Resource capacity leaks after repeated rare failures

A failed terminal job retains one reusable pool slot. Exhaustion requires several
independent terminal failures across reruns; the pool state is visible and can be
cleared manually during a supervised pilot.

- Factual status: the leak may be verified even when its exposure is uncertain.
- MVP relevance: bounded by expected job count, supervision, and recovery cost.
- Likely disposition: **Deferred hardening** with a trigger based on observed
  terminal failures or pool utilization.
- Escalate if one common failure can exhaust the pool or if recovery duplicates
  materially expensive work.

## Race requires future concurrency

Two workers can update the same record without a conditional write, but the MVP
has a single scheduled worker and no manual overlapping runs.

- Factual status: supported as a concurrency defect.
- MVP relevance: the precondition is outside the declared operating model.
- Disposition: **Deferred hardening** with reconsideration before concurrency is
  enabled.
- Do not claim current data corruption without evidence of overlapping writers.

## Throughput may be inadequate

The workflow performs one remote request per document. There is no measured
evidence that this misses the pilot window, and the pilot exists partly to learn
cost and duration.

- Factual status: verified implementation pattern; operational consequence is
  unknown.
- Disposition: **Pilot evidence**.
- Define metrics for duration, request cost, failure rate, and manual recovery;
  set a threshold that would trigger batching or parallelism work.

## Rare but irreversible corruption

A credible recovery path overwrites the only durable successful result before the
replacement is validated. The trigger is uncommon, but the lost result cannot be
reconstructed within the experiment window.

- Factual status: verify the overwrite ordering and lack of retained provenance.
- Disposition: **Merge blocker** when the path is reachable.
- “Operationally unlikely” is insufficient because one occurrence can silently
  invalidate expensive validation work.

## Prerequisite behind an enforced disabled path

The production role lacks a permission required by a new path, but an enforced
feature gate keeps that path disabled in every current environment.

- Factual status: verified against deployment policy and the activation guard.
- Disposition: **Rollout prerequisite** because merge cannot exercise the path.
- Author feedback: `suggestion(non-blocking):` with the exact enablement boundary.
- Reconsideration trigger: before enabling the path in an affected environment.

## Prerequisite on an active path

The same missing permission affects a path that the changed configuration enables
by default, with no deployment or runtime guard.

- Factual status: verified against deployment policy and active configuration.
- Disposition: **Rollout prerequisite** internally, but merge leaves the path
  activatable without containment.
- Author feedback: `issue(blocking):` because the prerequisite or an enforceable
  guard is required before merge.
- Focused validation: prove the required permission works or prove the path
  remains disabled in every environment until it does.

## Feature gate blocks fresh selection but not persisted work

A disabled feature gate prevents new records from being selected, but a partial
or recovery rerun can load records materialized while the gate was enabled.

- Factual status: verify both fresh selection and every persisted-input boundary.
- Disposition: **Rollout prerequisite** when no affected materialization exists
  before first activation and the disabled path cannot create one.
- Escalate to **Merge blocker** when affected state already exists or the gate is
  the declared rollback control for stopping previously selected work.
- Recovery: rematerialize the upstream selection or fail before side effects.
  Do not silently filter persisted records when an earlier stage may already
  have suppressed their fallback records; filtering can convert a visible
  failure into false absence.
- Focused validation: exercise a partial rerun with persisted enabled-path data
  while the gate is disabled.
