# CI pipeline secrets

The reusable workflow at `.github/workflows/ci.yml` needs the following secrets to
exist as **DocketProject1 organization secrets**, available to every service
repository via `secrets: inherit` in each caller workflow. Both are optional at the
workflow level (see `specs/001-reusable-ci-pipelines/contracts/ci-workflow-contract.md`)
so a repository can still get a build/test result without them, but the `analyze` and
`notify` jobs need them to do anything.

| Secret | Used by | Purpose |
|---|---|---|
| `SONAR_TOKEN` | `analyze` job | Authenticates `SonarSource/sonarqube-scan-action` against SonarQube Cloud |
| `CHAT_WEBHOOK_URL` | `notify` job | Incoming webhook URL for the pipeline-failure notification (Slack-compatible; also works for Discord and Microsoft Teams via their own webhook compatibility modes) |

No secret is ever printed, echoed, or written to a coverage/scan report artifact by
this pipeline. Neither secret is required for the `build-test` or `scan` jobs, which
is what keeps a fork pull request's build/test result usable (FR-017).

## Verifying they exist

```sh
gh secret list --org DocketProject1
```

Record only whether each name is present, never its value, in the pull request that
first depends on it.

**Status as of this feature's implementation**: not yet verified in this session —
verifying organization secrets requires an authenticated `gh` session with org-secret
read access, which this implementation pass did not have. This is called out
explicitly (rather than assumed) per the project's rule that a skipped verification is
reported as skipped, not silently treated as done. Confirm both secrets exist before
merging the `analyze` and `notify` jobs (T014, T028) into active use, or those jobs
will report `skipped` for every run.
