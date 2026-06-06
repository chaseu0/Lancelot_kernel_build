# Redmi 9 (`lancelot`) LineageOS Boot Builder

This scaffold is a GitHub Actions-oriented boot-image build repo for `LineageOS` on `Redmi 9 / lancelot`.

## What it is for

- Sync the public `LineageOS` source trees needed for `lancelot`
- Download a matching stock ROM archive for blob extraction
- Extract proprietary blobs into `vendor/...`
- Build only `boot.img`
- Upload the resulting `boot.img` and checksums as build artifacts

## What it does not guarantee

- Byte-for-byte reproducibility against an archived official `LineageOS` release
- Successful even-once boot-image builds on a standard GitHub-hosted runner

## Why a stock ROM URL is required

`lancelot` is not a fully blob-free target. Public source is available for the OS, device tree, and kernel tree, but the build still needs proprietary blobs. This scaffold expects a stock ROM archive so CI can extract the blobs instead of relying on a prebuilt vendor repo of unknown provenance.

## Runner expectations

Even a boot-only `LineageOS` sync + blob extraction + `bootimage` build is heavy. Treat `ubuntu-22.04` as a best-effort option for experimentation only.

For a real full-ROM build, prefer one of these:

- A self-hosted Linux runner with at least `250 GB` free disk
- GitHub larger runners if your plan supports them

## Files

- `.github/workflows/build-lineage-lancelot.yml`
- `manifests/lancelot-lineage-20.xml`

## Suggested first run

1. Create a new GitHub repo and push this scaffold.
2. Open `Actions`.
3. Run `Build boot.img for lancelot`.
4. Provide a stock ROM archive URL that matches the Android 11 firmware baseline expected by `lancelot`.
5. If the hosted runner runs out of space, switch the workflow to a self-hosted runner before trying again.

## Outputs

Successful runs upload an artifact named like:

- `boot-lancelot-<run-number>`

The artifact is intended to contain:

- `boot.img`
- `SHA256SUMS.txt`

## Safety notes

- Do not flash anything from CI until you have verified the artifact contents and hashes yourself.
- Do not assume a successful boot build means the `boot.img` matches an archived official release.
