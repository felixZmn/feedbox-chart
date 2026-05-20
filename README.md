# feedbox Helm Chart

[![CI](https://github.com/felixzmn/feedbox/actions/workflows/ci.yml/badge.svg)](https://github.com/felixzmn/feedbox/actions/workflows/ci.yml)
[![Release Helm Chart](https://github.com/felixzmn/feedbox/actions/workflows/release.yml/badge.svg)](https://github.com/felixzmn/feedbox/actions/workflows/release.yml)
[![GitHub Release](https://img.shields.io/github/v/release/felixzmn/feedbox)](https://github.com/felixzmn/feedbox/releases)
[![GHCR](https://img.shields.io/badge/ghcr.io-helm_chart-blue)](https://github.com/felixzmn/feedbox/pkgs/container/helm%2Ffeedbox)

Helm chart for deploying **feedbox**.

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

Releases are triggered by pushing a git tag that exactly matches the chart `version` in `Chart.yaml`.

### Steps

1. Update `version` in `Chart.yaml`
2. Update `appVersion` in `Chart.yaml` if the application version changed
3. Commit and push your changes
4. Create and push a git tag matching the chart version

Example:

```bash
git tag 1.2.3
git push origin 1.2.3
```

### What happens automatically

GitHub Actions will:

- run chart validation
- verify the git tag matches `Chart.yaml` `version`
- package the chart
- push the chart to GHCR
- create a GitHub Release

## CI

The CI workflow runs on pull requests and pushes to `main` and performs:

- `helm lint .`
- `helm template <chart-name> .`

## Versioning

- `version` in `Chart.yaml` is the **chart version**
- `appVersion` in `Chart.yaml` is the **application version**

For every release:

- bump `version`
- bump `appVersion` only if the app version changed
- create a matching git tag
