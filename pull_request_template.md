<!--
Organisation-wide default. Binding for every DocketProject1 repository.
All six sections are mandatory. Do not delete one: if it does not apply, write "N/A"
and one clause saying why.
Rules: docket-docs/pull-request-and-task-tracking-conventions.md
-->

## What changes

<!-- The behavioural difference, not a list of files. Git already shows the files. -->

## Why

<!-- The problem this solves. For a defect: what breaks today and what the user sees. -->

## Tasks

<!--
Task IDs this pull request advances, qualified by repository and spec, one per line:
  docket-iac specs/001-cluster-baseline T015
`tasks.md` is updated in THIS pull request, not a later one. Tick [X] only after
locating and inspecting the artifact each task names. Annotate partial delivery
instead of ticking it.
-->

## How it is verified

<!--
The exact commands run and the tail of their output, in a code block.
"Tests pass" is not verification. Never claim something you have not observed.
-->

```
```

## Risk and rollback

<!-- What can break, and the exact steps to undo this change. -->

## What this PR does not do

<!-- Deliberately excluded scope, so the reviewer does not go looking for it. -->

---

- [ ] Branch is `<type>/<kebab-summary>` and carries one concern
- [ ] The `test:` -> `feat:` commit pair is intact and not squashed, with the test committed failing
- [ ] `tasks.md` updated in this pull request
- [ ] Every `[X]` was ticked after locating and inspecting its named artifact
- [ ] Checks this change turned red are fixed; unrelated pre-existing failures are declared above
