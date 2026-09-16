# DocketProject1

A microservices TODO application (Go, Java, Node.js, Python, Vue) built out with
Terraform infrastructure, Kubernetes, Argo CD-based GitOps, and full CI/CD. The project
follows Spec-Driven Development (SDD): every change traces back to a specification, a
task, and a reviewed pull request.

This document is the starting point for anyone joining the team. Read it before cloning
anything.

## Repositories

| Repository | What it is |
| --- | --- |
| `docket-docs` | The constitution, delivery conventions, documentation style guide, and the SDD method itself. Read `constitution.md` first. |
| `docket-ai-agents` | The Spec-Driven Development hub: Spec Kit templates, scripts, agent skills, shared Claude Code settings, the pull request template and the conventions workflow. Every shared file is edited here and copied out from here. |
| `docket-auth-api` | Go service issuing JWTs. |
| `docket-users-api` | Spring Boot service serving user profiles. |
| `docket-todos-api` | Express service providing CRUD over TODO records. |
| `docket-log-message-processor` | Python worker consuming the TODO event queue. |
| `docket-frontend` | Vue single-page application. |
| `docket-iac` | Terraform infrastructure, plus the instructor's PC-IAC module governance rules. |
| `docket-gitops` | Kubernetes manifests and Argo CD configuration: the source of truth for deployed state. |
| `.github` | This repository. Organisation profile and the default pull request template. |

`microservice-app-example` is the original upstream training project the five
application services were forked from. It is historical reference only and is not part
of the actively developed set above.

## Getting started

1. **Clone the repositories you will work in.** Every repository carries its own copy of
   the Spec Kit skills, templates and the constitution, so a single clone works on its
   own. Clone them as siblings under one parent directory anyway: that layout is what
   the distribution script in step 5 needs, and it lets you read another repository's
   conventions without switching checkouts.

   ```
   mkdir docket && cd docket
   for r in docket-docs docket-ai-agents docket-auth-api docket-users-api \
            docket-todos-api docket-log-message-processor docket-frontend \
            docket-iac docket-gitops; do
     git clone git@github.com:DocketProject1/$r.git
   done
   ```

2. **Open the repository you are working in with Claude Code.** A `SessionStart` hook
   runs `.specify/scripts/bash/check-sdd-setup.sh` and tells the agent if anything is
   missing, so a broken checkout is visible rather than silent. Run that script yourself
   at any time to see the same result.

   The repository's `AGENTS.md` (copied to `CLAUDE.md` for older clients) opens with the
   rules that bind every session, then states that repository's stack, verified commands
   and structure.

3. **Read `docket-docs/constitution.md`.** It is binding and outranks any instruction
   given in a chat session. `docket-docs/pull-request-and-task-tracking-conventions.md`
   and `docket-docs/DOCUMENTATION_STYLE.md` cover commit, pull request, and
   documentation rules in detail.

4. **`docket-iac` has an additional step.** It combines the instructor's PC-IAC
   governance rules with the official `terraform` MCP server for current Terraform
   Registry documentation; both are required before authoring a module. See the
   "Terraform module governance" section of `docket-iac/AGENTS.md`. The MCP server is
   registered at project scope and needs a one-time approval the first time Claude Code
   opens that repository.

5. **Changing anything shared goes through the hub.** The agent skills, Spec Kit
   templates and scripts, the Claude Code settings, the pull request template and the
   conventions workflow are all authored in `docket-ai-agents` and copied into the other
   repositories. Edit them there, then:

   ```
   cd docket-ai-agents
   ./scripts/distribute-sdd.sh            # refresh every sibling repository
   ./scripts/distribute-sdd.sh --check    # report drift, write nothing
   ```

   The constitution is the same arrangement: authored in `docket-docs`, copied out by
   the same script. Editing a copy directly is drift, and `--check` is what catches it.

## Working under SDD

Every feature follows the cycle `specify -> clarify -> plan -> tasks -> implement ->
converge`, with human review after `specify` and after `plan`. Work with no task that
covers it gets a task first. Full detail is in `docket-docs/SPEC_DRIVEN_DEVELOPMENT.md`.

A pull request spanning more than one repository is opened as one pull request per
repository, each naming the others, merged in dependency order.

## What is enforced, and what is not

The organisation is on a plan without branch protection, so nothing on GitHub's side
stops a push to `main`. Branch protection and rulesets both return `403` and are not
purchasable on this plan. Four things stand in for it:

- A `PreToolUse` hook refuses a push to `main` or `master` from inside Claude Code.
- The `SDD conventions` workflow checks branch naming, Conventional Commits, the seven
  pull request sections, and the setup check on every pull request.
- The `Change management` workflow checks that a pull request declares its change class
  and risk, cites its request issue when the class is governed, and states a back-out
  path, with a rollback plan required when the risk is high.
- The `Release` workflow opens a Release pull request and stops. It has no merge step, so
  no tag and no release exist until a human merges it. That gate needs no branch
  protection, which is why it is the one enforcement here that cannot be waved through.

The first three are visible to the reviewer; they do not block a merge. Only the fourth
actually holds. None of them replaces review. An agent may open, describe and update a
pull request, and may never approve one, merge its own work, or author an acceptance
artifact.

The rules behind these checks are `docket-docs/CHANGE_MANAGEMENT.md` and
`docket-docs/RELEASE_MANAGEMENT.md`.
