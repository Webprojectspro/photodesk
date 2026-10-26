# PhotoDesk v1 verification — 2026-10-23

**Commit:** `5ed70efb` (+ this doc) on `feat/photodesk-rebrand`, forked from upstream `main` at `e6d6fb45` (Release 0.15.0).

## Build
- `packaging/macos/package.sh --arch aarch64` (ad-hoc signed, `MACOS_SIGN_IDENTITY` default) — succeeded end to end: build (`--locked --features heif`), bundle, codesign, DMG, CLI zip.
- Artifacts: `dist/release/PhotoDesk-0.15.0-macos-aarch64.dmg` (29 MB; copy of `photocraft-0.15.0-…` machine-named original), `photocraft-cli-0.15.0-macos-aarch64.zip`.
- Installed to `~/Applications/PhotoDesk.app`.

## Tests
- `cargo test --workspace --release --target aarch64-apple-darwin`: **947 tests passed across 22 suites; 1 failure**, `cms::builtins_encode_and_reparse` — the shipped ICC profile embedded the old description. Fixed by regenerating the profile (`PHOTOCRAFT_REGEN_PROFILES=1 … regen::`, commit `5ed70efb`); the cms suite then passes (33 passed, 1 ignored bench). No other failures; no logic changes were made anywhere, only branding strings/assets.

## Runtime checks
- `PhotoDesk.app` launches and opens a PNG; process runs from the installed bundle.
- `lsappinfo` reports `LSDisplayName = "PhotoDesk"`.
- `codesign -dv`: valid bundle signature, `Identifier=com.promotedge.photodesk`, arm64.
- Info.plist: `CFBundleIdentifier=com.promotedge.photodesk`, `CFBundleName=PhotoDesk`.
- Visual screenshot of the window was skipped (screen-capture access declined); the app was launched on screen for Benjamin to inspect directly.

## Branding sweep
- `grep -riE 'artcraft|getartcraft|ai\.storyteller'` over the tree, excluding license/attribution/acknowledgement files and this fork's docs, leaves only:
  - internal command ids (`help.artcraftWebsite`) and code comments — not user-visible;
  - stale rows in `i18n/*.tsv` for renamed labels — dead keys, never shown;
  - upstream user-manual markdown under `docs/user/`, `docs/releasing.md`, `docs/localization-zh-hans.md` — not shipped in the app bundle; rewrite when the PhotoDesk website/manual phase happens.
- `docs/brand/` (ArtCraft trademark assets) deleted; new PhotoDesk icon in `assets/app-icon/` (svg/png/icns/ico all regenerated).
- About box credits upstream; README and `docs/photodesk/ACKNOWLEDGEMENTS.md` carry the license attribution.

## Known deviations from the plan
- Upstream `packaging/macos/package.sh` was used instead of the planned `packaging/photodesk/make-app.sh` fallback — it already supports ad-hoc signing and builds the DMG, so the fallback was unnecessary.
- Smoke test used a PNG (no PSD at hand); PSD open is covered by the test suite's psd crate tests.
