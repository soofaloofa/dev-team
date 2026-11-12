---
name: dev-team
description: Spawns a small dev team (architect, implementor, reviewer) to design, implement, and review a task. Input is the task description and optionally a maximum number of review cycles (default 3).
---

# Dev Team

You are the **lead**. Spawn the team, hand over the task, answer
escalations, and report the outcome. The teammates coordinate with each
other directly; do not relay messages between them.

## Step 1: Resolve inputs

- **Task**: what to build.
- **N**: maximum review cycles. Use the number the user gave, else 3.
- **Base branch**: from `git symbolic-ref --short refs/remotes/origin/HEAD`,
  falling back to the current branch.

## Step 2: Create the shared workspace

```bash
mkdir -p .team/reviews
echo '.team/' >> "$(git rev-parse --git-path info/exclude)"
```

`.team/` holds the documents the team shares. It is never committed.

## Step 3: Spawn the team

Spawn three teammates with these exact names: `architect`, `implementor`,
`reviewer`. Give each the same run context plus a role line.

```
Run context
- Task: {task}
- Base branch: {base}
- Maximum review cycles N: {N}
- Lead: address messages to `lead`
- Teammates: architect, implementor, reviewer
```

Role lines:

- `architect`: design the task now and hand off per your instructions.
- `implementor`: wait for the architect's `PLAN READY`.
- `reviewer`: wait for the implementor's `REVIEW REQUEST`.

## Step 4: Handle what comes back

Silence is not completion. Act only on explicit messages.

| Message to you | Action |
| --- | --- |
| `VERDICT: APPROVED` from `reviewer` | Report to the user: branch name, summary, and that the review was automated. Do not merge. |
| `VERDICT: ESCALATE` from `reviewer` | Show the user the unresolved issues and ask whether to extend N, accept them, or abandon. Reply to `reviewer` with `LEAD DECISION: EXTEND N=<n>`, `ACCEPT`, or `ABANDON`. |
| Question or blocker from any teammate | Answer it, or ask the user. The teammate waits for your reply. |
| Reviewer idle with no verdict | Ask once for a verdict. If none comes, report the state to the user. |
