# review-pr-adversarially

An agent skill for evidence-backed adversarial PR reviews, risk calibration,
review comparisons, and deliberate publication of approved feedback.

Full reviews default to concurrent, clean-context **GPT-6.1 Sol + Grok** discovery
over the same complete, head-pinned diff and relevant callers. Kimi is not a
default lane. The outcome owner calibrates sealed findings after evidence
verification; all requested lanes and required relevant gates must complete
before final-complete publication. Comparisons and pending edits remain delta-scoped
unless changed behavior warrants a new full review. Explicit roster overrides
are supported; unavailable requested models are reported, never silently replaced.

## Source layout

The source lives in [`skills/review-pr-adversarially/`](skills/review-pr-adversarially/):

- `SKILL.md`: review workflow and invocation metadata.
- `references/`: blind orchestration, risk calibration, examples, portable ledger,
  and pending-review guidance.
- `agents/openai.yaml`: OpenAI agent display metadata.

This is an Agent Skills package; Claude Code can use its `SKILL.md` directly.
A separate Claude plugin is not required.

## Install

From any directory, using the installed [Skills CLI](https://github.com/vercel-labs/skills)
and [Socket Firewall](https://github.com/SocketDev/sfw-free):

```sh
sfw pnpm exec skills add chrisvaillancourt/review-pr-adversarially \
  --global --agent universal claude-code \
  --skill review-pr-adversarially --yes
```

This installs for universal agents **and** Claude Code. For universal discovery
only, use `--agent universal` instead of `--agent universal claude-code`.
This targets the shared skill directory, not every agent-specific directory.

Verified with Skills CLI 1.7.0. The command replaces any existing installation of
this skill. **Do not remove Claude's existing symlink first**: reinstalling works
with that link in place.

The default symlink mode creates this layout:

```text
repository/skills/review-pr-adversarially/     maintained source
~/.agents/skills/review-pr-adversarially/      installed copy
~/.claude/skills/review-pr-adversarially      symlink to the installed copy
```

The repository itself is **not** symlinked into the agent directories. Do not edit
the installed copy: reinstalling overwrites it. Installing from GitHub records
the remote source for update tracking.

Verify Claude's installation with:

```sh
sfw pnpm exec skills list --global --agent claude-code
```

Start a fresh agent session after installation if the current session caches its
skill list. In Claude Code, invoke `/review-pr-adversarially` with the PR or diff
and your review requirements.

> **FYI — telemetry:** The Skills CLI supports opting out through either
> `DISABLE_TELEMETRY` or `DO_NOT_TRACK`, set to `1` in your shell environment.
> Exporting either in your shell startup file makes the setting persistent for
> commands launched from that shell, including `sfw pnpm exec skills`. Other tools that
> honor the same variable are also affected. See the
> [Skills CLI telemetry documentation](https://github.com/vercel-labs/skills#telemetry).

## Local development

Edit the source under `skills/review-pr-adversarially/`, then install your changes
from the repository root:

```sh
sfw pnpm exec skills add . --global --agent universal claude-code \
  --skill review-pr-adversarially --yes
```

From another directory, replace `.` with the absolute path to your checkout.
Local-path installations do not provide a GitHub source for remote update
tracking. After developing locally, commit and publish the source updates to
GitHub, then reinstall from `chrisvaillancourt/review-pr-adversarially` using the
Install command above to restore remote tracking. Maintain this repository as
the source of truth; do not move the installed copy into the repository or treat
it as canonical.

Before publishing changes, review all files for credentials, personal data,
private project details, and internal links. The initial audit does not cover
future additions.
