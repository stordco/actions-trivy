# actions-trivy

Github Action for running trivy scans

# Overview

stordco/actions-trivy is used to run [trivy](https://github.com/aquasecurity/trivy) scans with various scan types. The current scan types supported:

1. [Filesystem](https://aquasecurity.github.io/trivy/v0.52/docs/target/filesystem/)
1. [Images](https://aquasecurity.github.io/trivy/v0.52/docs/target/container_image/)

# Filesystem scans

## Usage
<!-- {x-release-please-start-version} -->

```yaml
  - name: Trivy scan in fs mode
    uses: stordco/actions-trivy@v1.2.1
    with:
        scan-type: 'fs'
        github-token: ${{ secrets.GH_PERSONAL_ACCESS_TOKEN }}
        slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
```

<!-- {x-release-please-end} -->
## Inputs

| name | description | default value |
| --- | --- | --- |
| `github-token` | (optional) Should be set to `secrets.GH_PERSONAL_ACCESS_TOKEN` in order to interact with Github API. If not set, then PR comments will not be uploaded with the scan output. | "" |
| `scan-type` | (required) Specifies the type of scan to be performed (e.g., `fs` for filesystem scan). | |
| `slack-bot-token` | (optional) Should be set to `secrets.SLACK_BOT_TOKEN` to send messages through `Github Actions`. If not set, then slack messages will not be posted. | "" |
| `slack-channel-id` | (optional) Set to the desired Slack channel ID to receive alerts. If not set, then slack messages will not be posted. | "" |

## Outputs

| name | description | default value |
| --- | --- | --- |
| `artifact-url` | Returns link to trivy scan artifact. Main branch artifacts are retained for 90 days while others are retained for 1 day. | |

## General Information

For default usage:

1. When a merge into the `main` branch occurs that contains `CRITICAL` vulnerabilities, a notification will be sent to the `#trivy-alerts` Slack channel containing the number of critical vulnerabilities detected and a link to the full trivy scan report artifact.
1. When any vulnerabilities `(UNKNOWN, LOW, MEDIUM, HIGH, CRITICAL)` are detected on PR builds, a comment will be posted to the PR including the full output of the OS and library vulnerabilities detected based on the `mix.lock` dependencies.

# Image scans

## Usage
<!-- {x-release-please-start-version} -->
### Simple

```yaml
  - name: Trivy Image Scan
    uses: stordco/actions-trivy@v1
    with:
        scan-type: image
        image-ref: gcr.io/stord-ci/app-base:2024.06.25_d5cd08e
        github-token: ${{ secrets.GH_PERSONAL_ACCESS_TOKEN }}
        slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
        slack-channel-id: ${{ secrets.SLACK_SECURITY_ALERTS }}
```

### Matrix Jobs

```yaml
  - name: Trivy Image Scan
    uses: stordco/actions-trivy@v1
    with:
        scan-type: image
        image-ref: gcr.io/stord-ci/app-base:2024.06.25_d5cd08e
        matrix-id: unique-identifier
        github-token: ${{ secrets.GH_PERSONAL_ACCESS_TOKEN }}
        slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
        slack-channel-id: ${{ secrets.SLACK_SECURITY_ALERTS }}
```
<!-- {x-release-please-end} -->
## Inputs

| name | description | default value |
| --- | --- | --- |
| `github-token` | (optional) Should be set to `secrets.GH_PERSONAL_ACCESS_TOKEN` in order to interact with Github API. If not set, then PR comments will not be uploaded with the scan output. | "" |
| `image-ref` | (optional) Specifies the Docker image to be scanned | "" |
| `matrix-id` | (optional) If matrix jobs are being leveraged, add in an unique matrix job identifier to be leveraged for the notifications. | "" |
| `scan-type` | (required) Specifies the type of scan to be performed (e.g., `image` for container image scan). | |
| `slack-bot-token` | (optional) Should be set to `secrets.SLACK_BOT_TOKEN` to send messages through `Github Actions`. If not set, then slack messages will not be posted. | "" |
| `slack-channel-id` | (optional) Set to the desired Slack channel ID to receive alerts. If not set, then slack messages will not be posted. | "" |

## Outputs

| name | description | default value |
| --- | --- | --- |
| `artifact-url` | Returns link to trivy scan artifact. Main branch artifacts are retained for 90 days while others are retained for 1 day. | |

## General Information

For default usage:

1. When a merge into the `main` branch occurs that contains `CRITICAL` vulnerabilities, a notification will be sent to the `#trivy-alerts` Slack channel containing the number of critical vulnerabilities detected and a link to the full trivy scan report artifact.
1. When any vulnerabilities `(UNKNOWN, LOW, MEDIUM, HIGH, CRITICAL)` are detected on PR builds, a comment will be posted to the PR including the full output of the OS, library vulnerabilities and secrets detected found on the container image.

# Releasing

Releases are handled via `release-please`. Once a PR is merged, a new PR will be created that bumps all the versions. When that PR is merged the new release will be created and published for consumption.
