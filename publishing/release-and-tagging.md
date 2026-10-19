# Release And Tagging Runbook

Use this runbook to cut a release directly in a standalone platform repository
(for example `platform-workflows`, `platform-actions`, `platform-governance`,
or `platform-docs`). These repositories are the source of truth for their own
content; there is no monorepo subtree-publish step.

## Why This Matters

Reusable workflow callers usually reference a major tag such as `@v1`.

- `v1.2.0` style tags are immutable release snapshots.
- `v1` is a moving major alias.
- Callers on `@v1` only receive updates after `v1` is moved to a newer release commit.

## Prerequisites

1. Changes are committed and merged to `main` in the target repository.
2. You have permission to push tags on the target repository.

## Step 1: Prepare Target Repository Locally

GitHub.com example (`platform-workflows`):

```powershell
cd C:\Users\klee2\repos
git clone https://github.com/<org>/platform-workflows.git
cd .\platform-workflows
git fetch --all --tags
git checkout main
git pull
```

GHES example:

```powershell
git clone https://<ghes-host>/<org>/platform-workflows.git
```

## Step 2: Create Immutable Release Tag

Example with `v1.1.0`:

```powershell
git tag -a v1.1.0 -m "platform-workflows v1.1.0"
git push origin v1.1.0
```

## Step 3: Move Major Alias Tag

Move `v1` to latest release commit:

```powershell
git tag -fa v1 -m "Move v1 to v1.1.0"
git push origin v1 --force
```

## Step 4: Verify Tag References

GitHub.com:

```powershell
git ls-remote --tags https://github.com/<org>/platform-workflows.git
```

GHES:

```powershell
git ls-remote --tags https://<ghes-host>/<org>/platform-workflows.git
```

## Consumer Impact

- Repositories using `@v1` get updates after Step 3.
- Repositories pinned to exact tags (for example `@v1.0.0`) do not update automatically.

## Recommended Policy

1. Always create a new immutable tag for each release.
2. Move major alias tags intentionally and document the change.
3. Keep release notes that map major alias moves to immutable tags.
