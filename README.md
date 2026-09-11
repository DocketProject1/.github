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
| `docket-ai-agents` | The Spec-Driven Development hub: Spec Kit templates, scripts, and agent skills, shared by every other repository as symlinks. |
| `docket-auth-api` | Go service issuing JWTs. |
| `docket-users-api` | Spring Boot service serving user profiles. |
| `docket-todos-api` | Express service providing CRUD over TODO records. |
| `docket-log-message-processor` | Python worker consuming the TODO event queue. |
| `docket-frontend` | Vue single-page application. |
| `docket-iac` | Terraform infrastructure, plus the instructor's PC-IAC module governance rules. |
| `docket-gitops` | Kubernetes manifests and Argo CD configuration: the source of truth for deployed state. |
| `.github` | This repository. Organisation-wide pull request template and this README. |

`microservice-app-example` is the original upstream training project the five
application services were forked from. It is historical reference only and is not part
of the actively developed set above.

## Getting started

The five points below are not optional preamble: skipping them leaves an agent working
in a repository with no constitution, no Spec Kit skills, and no delivery rules loaded,
with no error to signal it.

1. **Clone every `docket-*` repository as a sibling under one parent directory.**
   Symlinks connecting them are relative and only resolve in this layout; a repository
   cloned on its own has broken links at `.specify/memory/constitution.md` and
   `.claude/skills`.

   ```
   mkdir docket && cd docket
   for r in docket-docs docket-ai-agents docket-auth-api docket-users-api \
            docket-todos-api docket-log-message-processor docket-frontend \
            docket-iac docket-gitops; do
     git clone git@github.com:DocketProject1/$r.git
   done
   ```

2. **Run the distribution script from the hub:**

   ```
   cd docket-ai-agents
   ./scripts/distribute-sdd.sh
   ```

   This links Spec Kit's templates, scripts, and agent skills from `docket-ai-agents`,
   and the constitution from `docket-docs`, into every other repository. It is
   idempotent; re-run it any time a repository is added or re-cloned.

3. **Read `docket-docs/constitution.md`.** It is binding and outranks any instruction
   given in a chat session. `docket-docs/pull-request-and-task-tracking-conventions.md`
   and `docket-docs/DOCUMENTATION_STYLE.md` cover commit, pull request, and
   documentation rules in detail.

4. **Open the repository you are working in with Claude Code.** Its `AGENTS.md`
   (symlinked as `CLAUDE.md`) states that repository's stack, verified commands, and
   structure, and the `/speckit-*` skills become available once the symlinks from step 2
   are in place.

5. **`docket-iac` has an additional step.** It combines the instructor's PC-IAC
   governance rules with the official `terraform` MCP server for current Terraform
   Registry documentation; both are required before authoring a module. See the
   "Terraform module governance" section of `docket-iac/AGENTS.md`. The MCP server is
   registered at project scope and needs a one-time approval the first time Claude Code
   opens that repository.

## Working under SDD

Every feature follows the cycle `specify -> clarify -> plan -> tasks -> implement ->
converge`, with human review after `specify` and after `plan`. Work with no task that
covers it gets a task first. Full detail is in `docket-docs/SPEC_DRIVEN_DEVELOPMENT.md`.

A pull request spanning more than one repository is opened as one pull request per
repository, each naming the others, merged in dependency order.
