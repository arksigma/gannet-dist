<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/brand/gannet-horizontal-logo-on-dark.svg">
    <img alt="Gannet" src="assets/brand/gannet-horizontal-logo.svg" width="420">
  </picture>
</p>

# Gannet releases

Release artifacts for [Gannet](https://github.com/arksigma/gannet), a desktop
API client with WebAssembly viewer plugins and an S3 browser that reads Bruno
collections.

Install via Homebrew (macOS and Linux):

```sh
brew trust --cask arksigma/tap/gannet
brew install --cask arksigma/tap/gannet
```

The macOS app is signed with a Developer ID and notarised by Apple. On Linux,
`gannet-app --install-desktop-entry` adds the application-menu entry and icons
that a cask cannot install. Gannet updates itself from the `latest.json`
manifest attached to each release here (signed; see
[updates](https://github.com/arksigma/gannet/blob/master/docs/install.md#updates)),
so the cask is marked `auto_updates`.

Or grab a tarball from the [releases](https://github.com/arksigma/gannet-dist/releases)
(`darwin`/`linux` × `x86_64`/`arm64`; checksums in `checksums.txt`). Each one
holds `gannet` (CLI), `gannet-app` (on macOS inside `Gannet.app`), the reference
viewer plugins under `plugins/<id>/` and the licences.

The release workflow in this repo builds the source at a given tag on GitHub's
free public-repo runners and publishes here — triggered automatically by tag
pushes on the source repo, or manually:

```sh
gh workflow run release.yml -R arksigma/gannet-dist -f tag=vX.Y.Z
```

## Setting this repo up

1. Create the public repo `arksigma/gannet-dist` with this README and
   `.github/workflows/release.yml` (both live under `packaging/dist-repo/` in the
   source repo; copy them, do not edit the copies).
2. Secrets on **gannet-dist**: `RELEASE_GITHUB_TOKEN` (repo scope including
   `arksigma/gannet`, used to check out the source at the tag),
   `HOMEBREW_TAP_GITHUB_TOKEN` (repo scope on `arksigma/homebrew-tap`) and
   `UPDATER_SIGNING_KEY` (ed25519 private key, PKCS#8 PEM, that signs
   `latest.json`; its public half is compiled into the app — see
   `docs/updater.md` in the source repo).
3. Secret on **gannet** (source): `RELEASE_GITHUB_TOKEN` with permission to send
   a `repository_dispatch` to `gannet-dist`.
4. Optional, for signed/notarised macOS builds: `APPLE_SIGNING_IDENTITY`,
   `APPLE_CERTIFICATE`, `APPLE_CERTIFICATE_PASSWORD`, `APPLE_ID`, `APPLE_TEAM_ID`,
   `APPLE_PASSWORD` on gannet-dist. Without them the app is ad-hoc signed.
