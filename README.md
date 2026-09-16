# DocketProject1/.github

Organization-wide defaults and shared automation for DocketProject1: the profile
README (`profile/README.md`), the default pull request template
(`pull_request_template.md`), and the reusable CI pipeline described below.

## Reusable CI pipeline

`.github/workflows/ci.yml` is a `workflow_call` reusable workflow that every Docket
service repository calls from its own thin `.github/workflows/ci.yml`, instead of
each repository duplicating its own build/test/analysis/publish logic. Full contract
— exact inputs, secrets, outputs, and the guarantees it makes to every caller — is
`specs/001-reusable-ci-pipelines/contracts/ci-workflow-contract.md` in the
`docket-ai-agents` repository (that repository owns this feature's spec, plan and
tasks; this repository owns the implementation the spec points at).

**What it does, per run**: builds the calling service the way it will be packaged
(its own Dockerfile), runs its existing test command, runs SonarQube Cloud analysis
and a Trivy vulnerability scan of the built image as quality gates, and — only on a
release tag that `release.yml` (`release-please`) produced in the calling repo, after
every gate passed — publishes an image to GHCR tagged with that release version. It
never deploys anything, on any trigger, and it never computes its own version:
`release.yml` in each service repo is the only source of truth for that.

**Adding a sixth service**: add one `if: inputs.language == '<lang>'` block of setup/
build/test steps to `ci.yml`'s `build-test` job (each existing language's block is
self-contained and a template for the next one), and give the new service repository
its own thin caller workflow per the contract's "Called as" example — including the
`push.tags: ["v*"]` trigger, or it will never reach the publish job. Steps are inlined
directly in `ci.yml` rather than factored into composite actions under
`.github/actions/`, because a step's `uses:` cannot reference an action in this same
repository with an expression, and a hardcoded second ref invites drift; see
`specs/001-reusable-ci-pipelines/tasks.md` (T008's history) for what didn't work. See
`docs/ci-secrets.md` for the organization secrets the pipeline expects.

Required secrets: `docs/ci-secrets.md`.
