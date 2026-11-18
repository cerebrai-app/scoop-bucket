# scoop-bucket

[Scoop](https://scoop.sh) manifests for the cerebrai command line tools on Windows (x64),
built from [cerebrai-app/go-cerebrai](https://github.com/cerebrai-app/go-cerebrai).

| Manifest | What it is |
| --- | --- |
| [`brainctl`](brainctl.json) | The brainctl command line interface |
| [`workctl`](workctl.json) | The workctl command line interface |

## Install

Add this repository as a bucket, then install from it:

```powershell
scoop bucket add cerebrai https://github.com/cerebrai-app/scoop-bucket
scoop install cerebrai/brainctl
scoop install cerebrai/workctl
```

`cerebrai` is the local name for the bucket, and `cerebrai/` installs from it even if
another bucket has a manifest with the same name.

Scoop has no separate trust step: adding the bucket is the decision to trust it. Scoop
checks each download against the SHA-256 hash in the manifest before installing it.

Check that it worked:

```powershell
brainctl version
workctl version
```

## Update and uninstall

```powershell
scoop update
scoop update brainctl workctl
```

To remove the tools and the bucket:

```powershell
scoop uninstall brainctl workctl
scoop bucket rm cerebrai
```

## Maintaining

GoReleaser writes the manifests when go-cerebrai publishes a release (see
`go-release.yml` in [cerebrai-app/actions](https://github.com/cerebrai-app/actions)), so
don't edit them by hand. Change `.goreleaser.yaml` in go-cerebrai instead.

This repository's workflows call the shared actions in
[cerebrai-app/actions](https://github.com/cerebrai-app/actions) and run on the
`self-hosted` runners:

| Workflow | What it does |
| --- | --- |
| [`claude.yml`](.github/workflows/claude.yml) | Runs Claude Code when someone mentions `@claude` |
| [`claude-code-review.yml`](.github/workflows/claude-code-review.yml) | Reviews every PR with the `code-review` and ponytail plugins |
| [`renovate.yml`](.github/workflows/renovate.yml) | Weekly self-hosted Renovate, configured by [`renovate.json`](renovate.json) to keep pinned actions current |

They need these secrets, set at the repository or organization level:

- `CLAUDE_CODE_OAUTH_TOKEN` for the Claude workflows.
- `RENOVATE_APP_CLIENT_ID` and `RENOVATE_APP_PRIVATE_KEY` for Renovate. Until they exist,
  the job only prints a warning.
