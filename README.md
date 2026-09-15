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
trunk push that passed every gate — publishes a uniquely versioned image to GHCR. It
never deploys anything, on any trigger.

**Adding a sixth service**: create a composite action under `.github/actions/` if the
new service's language isn't one of the five already supported
(`setup-go`, `setup-java`, `setup-node`, `setup-python`, `setup-static-site`), add one
`case`/`if` branch to `ci.yml`'s `build-test` job dispatching to it, and give the new
service repository its own thin caller workflow per the contract's "Called as"
example. See `docs/ci-secrets.md` for the organization secrets the pipeline expects.

Required secrets: `docs/ci-secrets.md`.
