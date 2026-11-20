# camofox-umbrel

Build pipeline for the **iconicfinesse-camofox** Umbrel app — a self-hosted
anti-detection browser backend for Hermes Agent (Iris) on Brauk's UmbrelOS.

## What this repo does

A GitHub Actions workflow (`.github/workflows/build.yml`) checks out
[jo-inc/camofox-browser](https://github.com/jo-inc/camofox-browser) at the
commit pinned in `UPSTREAM_SHA`, builds its self-contained `Dockerfile.ci`
(which downloads the pinned Camoufox Firefox fork + yt-dlp), and pushes the
image to `ghcr.io/iconicfinesse/camofox-browser`.

The Umbrel app package itself lives in the
[iconicfinesse-umbrel-app-store](https://github.com/IconicFinesse/iconicfinesse-umbrel-app-store)
repo under `iconicfinesse-camofox/`, referencing this image by digest.

## Updating

1. Edit `UPSTREAM_SHA` to the new upstream commit and bump `IMAGE_VERSION`
   in the workflow to match.
2. Push — the workflow rebuilds and republishes the image.
3. Update the digest + version in the store package and update the app.

Updates are elective: the app keeps running on its pinned image until then.

## Design notes

- The browser runs **anonymously** — no imported cookies, no account logins.
- Vendor crash telemetry is disabled in the app package
  (`CAMOFOX_CRASH_REPORT_ENABLED=false`).
- Upstream: jo-inc/camofox-browser (MIT), wrapping Camoufox (Firefox fork
  with C++-level fingerprint spoofing).
