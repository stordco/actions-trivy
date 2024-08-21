# actions-trivy
Github Action for running trivy scans

# Overview
stordco/actions-trivy is used to run [trivy](https://github.com/aquasecurity/trivy) scans with various scan types. The current scan types supported:
1. [Filesystem](https://aquasecurity.github.io/trivy/v0.52/docs/target/filesystem/)


# Filesystem scans
## Usage
<!-- {x-release-please-start-version} -->

```yaml
  - name: Trivy scan in fs mode
    uses: stordco/actions-trivy@v1.1.2
    with:
        scan-type: 'fs'
        github-token: ${{ secrets.GH_PERSONAL_ACCESS_TOKEN }}
        slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
```

## Inputs
<!-- {x-release-please-end} -->

| name | description | default value |
| --- | --- | --- |
| `scan-type` | (required) Specifies the type of scan to be perforemed (e.g., `fs` for filesystem scan). | |
| `github-token` | (optional) Should be set to `secrets.GH_PERSONAL_ACCESS_TOKEN` in order to interact with Github API. If not set, then PR comments will not be uploaded with the scan output. | "" |
| `slack-bot-token` | (optional) Should be set to `secrets.SLACK_BOT_TOKEN` to send messages through `Github Actions`. If not set, then slack messages will not be posted. | "" |


## Outputs

| name | description | default value |
| --- | --- | --- |
| `artifact-url` | Returns link to trivy scan artifact. Main branch artifacts are retained for 90 days while others are retained for 1 day. | |


## General Information

For default usage:
1. When a merge into the `main` branch occurs that contains `Critical` vulnerabilities, a notification will be sent to the #trivy-alerts Slack channel containing the number of critical vulnerabilities detected and a link to the full trivy scan report artifact.
1. When any vulnerabilities `(UNKNOWN, LOW, MEDIUM, HIGH, CRITICAL)` are detected on PR builds, a comment will be posted to the PR including the full output of the OS vulnerabilities detected based on the `mix.lock` dependencies.

# Releasing

Releases are handled via `release-please`. Once a PR is merged, a new PR will be created that bumps all the versions. When that PR is merged the new release will be created and published for consumption.