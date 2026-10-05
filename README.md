# Runlot Helm chart repository

This repository is the Helm chart index behind `https://charts.runlot.io`. It holds nothing
but the packaged charts and the `index.yaml` that points at them; the chart's source lives in
[`runlot/runlot`](https://github.com/runlot/runlot) under `charts/runlot`, and every chart
here is one release of that repository, version for version.

```sh
helm repo add runlot https://charts.runlot.io
helm repo update
helm search repo runlot --versions
helm install runlot runlot/runlot -n runlot --create-namespace -f values.yaml
```

The install guide is at <https://docs.runlot.io/docs/self-hosted>. The chart's annotated
`values.yaml`, its `values.schema.json`, and the NOTES it prints after an install are inside
each package: `helm show values runlot/runlot --version <v>`.

## How a chart gets here

The `release.yml` workflow in `runlot/runlot` packages `charts/runlot` on every `v*` tag,
uploads the `.tgz` to that tag's GitHub Release, and then commits the same file into this
repository's `charts/` directory and regenerates `index.yaml` with `helm repo index --merge`.
Nothing is pushed here by hand, and a chart that is in the index is byte-identical to the one
on the Release — the Release holds the `SHA256SUMS` and its signature for the air-gap bundle
(`runlot-license bundle-verify`).

GitHub Pages serves `main` as-is at `runlot.github.io/charts`; `charts.runlot.io` is a
CNAME to it.

The index is built with `helm repo index . --url https://charts.runlot.io`: the URL is the
site root, and helm prefixes it to each chart's path under `charts/`.

## Versions

Chart `version` and `appVersion` are the runlot tag without the leading `v`
(`0.1.0-rc.110` for `v0.1.0-rc.110`). A prerelease version is not shown by
`helm search repo` unless you pass `--devel`; while every runlot release is a release
candidate, that flag is needed:

```sh
helm search repo runlot --devel
helm install runlot runlot/runlot --devel -n runlot -f values.yaml
```
