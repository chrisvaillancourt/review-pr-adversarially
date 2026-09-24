# GitHub pending reviews

Use this workflow when reading, editing, or publishing a GitHub review whose
state is `PENDING`.

## Read the complete draft

GitHub's global pull-request comments endpoint may omit draft comments. Read the
pending review and its comments through review-scoped REST endpoints or GraphQL
review threads.

Before mutation, capture:

- pull-request head revision and aggregate review decision;
- pending review ID, node ID, state, body, commit, and submission time;
- every draft comment's ID, body, file, diff side, start line, end line, and
  outdated state; and
- the current draft comment count.

The audit is complete when every draft comment maps to one calibrated candidate
and one current diff anchor.

## Mutate the pending review

Use GraphQL `updatePullRequestReviewComment` to edit a draft comment and
`addPullRequestReviewThread` to add a new inline thread to the existing pending
review. A REST update may return `404` for a draft comment even when
review-scoped reads can see it.

Keep the review body empty when all feedback is inline. Mutation authorization
covers the requested draft changes and readback only; keep the review pending
until the user explicitly authorizes publication.

After an ambiguous failure, re-read the pending review before retrying so a
successful mutation is not duplicated.

## Read back each mutation

Verify through review-scoped REST and GraphQL data:

- review state is still `PENDING` and submission time is null;
- review commit matches the current pull-request head;
- review body has the intended content;
- comment count matches the expected draft;
- every comment body exactly matches the approved text;
- every comment is anchored to the intended file, diff side, and line range; and
- every thread is current rather than outdated.

When a draft-comment REST response has null line fields, confirm the effective
line range through the GraphQL review thread.

## Publish and read back

Publication remains a separate authorized operation. Before submission, re-read
the current head, draft comments, inline severities, and aggregate pull-request
review decision.

After submission, verify the submitted review state and the aggregate
pull-request review decision separately. A `COMMENT` submission does not clear
an earlier `CHANGES_REQUESTED` decision.