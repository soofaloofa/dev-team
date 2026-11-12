---
name: pdd
description: Prompt-Driven Development. Turns a rough idea into a single design document with requirements, research, design, and an ordered implementation plan. Use when you need a design that another engineer can implement without asking questions.
---

# PDD (Prompt-Driven Development)

Produce **one document**, `<run dir>/design.md`, that is the source of truth
for the task. Everyone downstream reads it instead of asking you.

## Parameters

- `task` (required): the rough idea.
- `mode`: `interactive` (default) asks the user questions; `auto` answers
  its own questions from research. Use `auto` when no human is present.
- `run dir`: directory for the document. Defaults to `.team` when omitted.

## Process

Work through the stages in order. Write each section into the document as
you finish it, so the file is always the current state of your thinking.

### 1. Requirements

Restate the task. List what is in scope, what is out of scope, and the
acceptance criteria. In `interactive` mode, ask the user one question at a
time until the criteria are unambiguous. In `auto` mode, resolve each
question by reading the code, and record the answer and the evidence.

### 2. Research

Read the codebase. Find the closest existing feature and note its file
paths, patterns, and test style. Record what you found and what you
decided to mirror. Do not guess when you can look.

### 3. Design

Describe the change: components touched, interfaces, data flow, and error
handling. For each non-obvious choice, record the decision, the reason, and
the alternative you rejected.

### 4. Implementation plan

An ordered checklist. Each step is small enough to implement and test on
its own, names the files it touches, and says how it is tested. Earlier
steps must not depend on later ones.

## Document layout

```markdown
# <Title>

## Requirements
## Research
## Design
### Decisions
- <decision>: <choice> because <reason>. Rejected: <alternative>.
## Plan
- [ ] Step 1: ...
```

## Rules

- Solve what was asked. Note future ideas under "Out of scope".
- Be specific. "Add a `summarize()` method to `TaskService`, following
  `ReportService.summarize()`" is a plan; "implement the service" is not.
- Every decision traces to something you found or something the user said.
