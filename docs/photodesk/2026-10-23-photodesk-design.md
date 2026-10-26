# PhotoDesk v1 — Design

**Date:** 2026-10-23
**Owner:** Benjamin Sardar (PromotEdge)
**Status:** Approved

## Goal

Ship **PhotoDesk**, Benjamin's own macOS desktop photo editor, as a product under his own brand. v1 is a rebranded fork of [storytold/photocraft](https://github.com/storytold/photocraft) — an open-source Photoshop-class editor written in Rust (egui UI, wgpu GPU compositor, PSD support, layers, brushes, filters).

## Why fork photocraft

- Dual-licensed **MIT / Apache-2.0**: forking, rebranding and commercial distribution are permitted.
- 71k-star, actively developed codebase — years of head start over building from scratch.
- Cross-platform (macOS, Windows, Linux, web/WASM), so later platform expansion is already solved upstream.

**License obligations:** keep upstream LICENSE files and copyright notices; remove ArtCraft trademarks (logos/wordmarks in `docs/brand/` and the app icon). Credit photocraft in an acknowledgements/About section.

## Scope of v1 (rebrand + ship)

1. **Fork** `storytold/photocraft` to Benjamin's GitHub, clone to `~/src/photodesk`, work on branch `feat/photodesk-rebrand`.
2. **Rebrand** every user-visible surface:
   - App name: **PhotoDesk** (window title, menu bar, About box, strings).
   - App/bundle id: `com.promotedge.photodesk` → `com.promotedge.photodesk`.
   - New app icon and any splash/branding assets (ArtCraft marks removed).
   - Cargo package metadata (name, authors, homepage) where it feeds the UI/bundle.
   - Acknowledgements entry crediting photocraft/ArtCraft with license text retained.
3. **Build** a release macOS build and package it as `PhotoDesk.app`.
4. **Verify**: `cargo test --workspace` passes; app launches, opens PSD/PNG/JPEG, basic editing works; no ArtCraft branding visible anywhere in the UI.

**Deliverable:** a runnable, fully rebranded `PhotoDesk.app` on Benjamin's Mac (unsigned; opened via right-click → Open).

## Environment & constraints

- **Repo location:** `~/src/photodesk` — never inside OneDrive (cargo/git in a syncing folder causes corruption). This OneDrive "Adobe Apps" folder holds only docs and branding assets.
- **Rust toolchain:** must be reinstalled (`rustup` was removed in the 2026-10 disk cleanup): `rustup toolchain install stable` (~1.5 GB).
- **Disk:** only ~13 GB free on the Mac. Build release-only (no debug builds), run `cargo clean` of unneeded artifacts, and free space first if the checkout + `target/` won't fit. Known offload candidates exist from the October cleanup.

## Out of scope for v1 (later phases)

- New features or code changes beyond branding.
- AI integrations (background removal, generative fill).
- Apple code signing / notarization and DMG distribution.
- Windows/Linux builds (later, via GitHub Actions).
- Website and public launch.
- Keeping in sync with upstream (the fork makes this possible; doing it is a later task).

## Risks

- **Disk space** is the main build risk — mitigated by pre-flight space check and release-only builds.
- **Build breakage** on latest upstream — mitigate by building from the latest release tag rather than `main` if `main` fails.
- **Name collisions:** "PhotoDesk" has been used by other software historically; fine for a v1 app name, worth a trademark check before public launch.
