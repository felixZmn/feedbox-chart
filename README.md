<p align="center">
  <img src="https://github.com/felixZmn/feedBox/blob/main/src/main/resources/META-INF/resources/icons/package.svg" alt="FeedBox Logo" width="100"/>
</p>

<h1 align="center">FeedBox Helm Chart</h1>
<h3 align="center">Put all your feeds into a box!</h3>

<p align="center">
  <!-- CI -->
  <a href="https://github.com/felixzmn/feedbox-chart/actions/workflows/ci.yml">
    <img src="https://github.com/felixzmn/feedbox-chart/actions/workflows/ci.yml/badge.svg" alt="CI Status"/>
  </a>
  <!-- Release -->
  <a href="https://github.com/felixzmn/feedbox-chart/actions/workflows/release.yml">
    <img src="https://github.com/felixzmn/feedbox-chart/actions/workflows/release.yml/badge.svg" alt="Release Status"/>
  </a>
  <!-- Latest Release -->
  <a href="https://github.com/felixzmn/feedbox-chart/releases">
    <img src="https://img.shields.io/github/v/release/felixzmn/feedbox-chart?logo=github" alt="Latest Release"/>
  </a>
  <!-- Helm Chart -->
  <a href="https://github.com/felixzmn/feedbox-chart/pkgs/container/helm%2Ffeedbox">
    <img src="https://img.shields.io/badge/helm-chart-blue?logo=helm" alt="Helm Chart"/>
  </a>
  <!-- License -->
  <a href="./LICENSE">
    <img src="https://img.shields.io/github/license/felixzmn/feedbox-chart" alt="License"/>
  </a>
</p>

Helm chart for deploying **FeedBox**.

## Chart repository

This chart is published as an OCI artifact to GitHub Container Registry:

```bash
oci://ghcr.io/felixzmn/helm
```

## Installation

```bash
helm install feedbox oci://ghcr.io/felixzmn/helm/feedbox --version <version>
```

Example:

```bash
helm install feedbox oci://ghcr.io/felixzmn/helm/feedbox --version 1.2.3
```

## Upgrade

```bash
helm upgrade feedbox oci://ghcr.io/felixzmn/helm/feedbox --version <version>
```

## Pull chart locally

```bash
helm pull oci://ghcr.io/felixzmn/helm/feedbox --version <version>
```

## Development

Validate the chart locally:

```bash
helm lint .
helm template feedbox . > /dev/null
```

Package manually if needed:

```bash
helm package .
```

Push manually if needed:

```bash
helm push feedbox-*.tgz oci://ghcr.io/felixzmn/helm
```

## Release process

Chart releases are driven by `Chart.yaml` `version` on `main`. After merge, the release workflow creates the matching git tag, pushes the OCI chart to GHCR, and creates a GitHub Release.

| Change | Who | What happens |
|--------|-----|----------------|
| App image patch/minor | Renovate (automerge) | Updates `appVersion`, bumps chart `version`, merges → release |
| App image major | You merge the Renovate PR | Same publish path after merge |
| Chart feature / breaking | You bump `version` in the PR and merge | Release on merge |

### Feature / manual chart change

1. Edit templates/values as needed
2. Bump `version` in `Chart.yaml` (and `appVersion` only if the app image changed)
3. Open a PR — CI runs `helm lint` / `helm template`
4. Merge to `main` → release workflow publishes

### What the release workflow does

- skips if a git tag for the current chart `version` already exists
- runs `helm lint` and `helm template`
- creates annotated tag `$version`
- packages and pushes to `oci://ghcr.io/felixzmn/helm`
- creates a GitHub Release

### Required secrets

- `RENOVATE_PAT` — for the Renovate workflow
- `GHCR_TOKEN` — so Renovate can read `ghcr.io/felixzmn/docker/feedbox` tags

Also protect `main` and require the **CI** check so Renovate only automerges green PRs.

## CI

The CI workflow runs on pull requests and performs:

- `helm lint .`
- `helm template <chart-name> .`

## Versioning

- `version` in `Chart.yaml` is the **chart version** (release tag / OCI chart version)
- `appVersion` in `Chart.yaml` is the **application image tag** (used when `image.tag` is empty)
