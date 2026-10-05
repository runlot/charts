# Runlot Helm chart repository

This repository is the Helm chart index behind `https://charts.runlot.io`. It holds nothing
but the packaged charts and the `index.yaml` that points at them. The chart's source is part
of the runlot product repository, which is private; what you read is the package itself.

```sh
helm repo add runlot https://charts.runlot.io
helm repo update
helm search repo runlot --devel
helm install runlot runlot/runlot --devel -n runlot --create-namespace -f values.yaml
```

**A licence is required.** The chart installs nothing without the Secret that holds the
licence file we issue per installation, and the master key you generate; we keep a copy of
neither. Ask for one at <https://runlot.io/en/contact>. An evaluation licence is free for
90 days on a single machine.

The install guide is at <https://docs.runlot.io/docs/self-hosted/helm>. The chart's
annotated `values.yaml`, its `values.schema.json`, and the NOTES it prints after an install
are inside each package:

```sh
helm show values runlot/runlot --devel
helm pull runlot/runlot --devel --untar
```

## How a chart gets here

Runlot's release pipeline packages the chart on every product tag, commits the `.tgz` into
this repository's `charts/` directory and regenerates `index.yaml` with
`helm repo index --merge`. Nothing is pushed here by hand. The same file ships inside the
air-gap bundle we send with a licence, beside `SHA256SUMS` and its detached signature, so a
chart pulled from here can be checked against that bundle offline with
`runlot-license bundle-verify` (see
[Air-gap](https://docs.runlot.io/docs/self-hosted/operate#air-gap)).

GitHub Pages serves `main` as-is; `charts.runlot.io` is a CNAME to it. The index is built
with `helm repo index . --url https://charts.runlot.io`: the URL is the site root, and helm
prefixes it to each chart's path under `charts/`.

## Versions

Chart `version` and `appVersion` are the runlot tag without the leading `v`
(`0.1.0-rc.110` for `v0.1.0-rc.110`). A prerelease version is not shown by
`helm search repo` unless you pass `--devel`; while every runlot release is a release
candidate, that flag is needed.
