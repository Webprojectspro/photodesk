# Rebrand inventory (generated 2026-10-23)

Grep base: `grep -rniE 'photocraft|artcraft|storyteller|storytold|getartcraft'` → 18,666 hits, almost all crate-internal identifiers that stay.

## A. User-visible display strings — `PhotoCraft` → `PhotoDesk`
- 165 `.rs` files contain the literal `PhotoCraft` (window title in `crates/ui-egui/src/panels.rs`, `APP_NAME` in `crates/ui-egui/src/native_menu.rs:31`, About in `crates/ui-egui/src/dialogs.rs:649`, splash in `crates/ui-egui/src/canvas.rs:3199`, plus matching test asserts). Replace case-sensitively in **code and tests together** so the suite stays consistent.
- `crates/ui-egui/src/i18n/*.tsv` — translations are keyed by the English source string; replace `PhotoCraft` there too so lookups keep matching. `LICENSE-translations.txt` stays untouched (license).

## B. App/bundle id — `ai.storyteller.photocraft` (old) → `com.promotedge.photodesk`
- `packaging/macos/Info.plist.in` (bundle id + document UTIs), `packaging/macos/package.sh` (CLI signing id).
- `packaging/linux/*` (.desktop, AppRun, flatpak manifests, mime/metainfo) + file renames; `packaging/icons.sh`; `packaging/windows/package.ps1`; icon filenames under `assets/app-icon/hicolor/*/apps/`.

## C. Icons & visual assets — replace
- `assets/app-icon/`: `photocraft.svg`, `photocraft-small.svg`, `photocraft-1024.png`, `photocraft.icns`, `photocraft.ico`, `hicolor/` PNG set. New PhotoDesk mark, regenerated via `packaging/icons.sh` logic + `iconutil`.
- `packaging/macos/dmg/background.svg|tiff` — carries PhotoCraft branding (later-phase DMG; replace text if trivial, else note).

## D. Trademark files — delete
- `docs/brand/` (ArtCraft logos/wordmarks, LICENSE-brand.txt explicitly restricts trademark use).

## E. Links, docs, metadata
- `crates/ui-egui/src/links.rs`: APP_PAGE/GITHUB/ISSUES → `github.com/Webprojectspro/photodesk`; DISCORD → fork discussions; ARTCRAFT_WEBSITE → promotedge.com; adjust labels ("ArtCraft" → "PromotEdge") where they surface.
- `README.md`: replace with a short PhotoDesk README crediting upstream. `AGENTS.md`: rewrite branding paragraph for PhotoDesk.
- Acknowledgements: new `docs/photodesk/ACKNOWLEDGEMENTS.md` + credit line in About dialog.

## F. Must NOT change
- `LICENSE-APACHE`, `LICENSE-MIT`, `NOTICE`, `ATTRIBUTION.md`, `i18n/LICENSE-translations.txt`, copyright headers.
- Crate names (`photocraft-*`), Rust identifiers (`PhotocraftApp`), lowercase machine ids inside code, `Cargo.lock` dependency names, upstream URLs in code comments.
- Test fixture bytes like `b"PhotoCraftCo"`… are replaced only because tests build and assert from the same literal (self-consistent); no logic changes anywhere.
