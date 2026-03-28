# Changelog

All notable changes to this project will be documented here.

## [0.4.0] - 2026-03-28

### Changed

- TCA themes are now standard [base24](https://github.com/tinted-theming/base24/) YAML files (`.yaml`). The custom TOML format is retired
- Spec rewritten around base24 as the canonical file format
- Semantic mapping tables now reference base24 slot names directly
- Added Library Conformance section with explicit MUST/SHOULD/MAY requirements
- `meta.dark` now derived from base24 `variant` field
- `tca export` removed — use [Tinted Theming](https://github.com/tinted-theming/home) tooling for export
- Removed JSON validation schema (superseded by base24 schema)
- All previous TOML themes removed

## [0.3.0] - 2026-03-16

### Changed

- Removed `slug` field from theme metadata
- Added XDG file location and user preferences specification
- Extended base16 support to base24 (`base00`–`base17`)

## [0.2.0] - 2026-03-17

### Changed

- Extended base16 support to include base24
- Added theme and preferences file location specification
- Linked to external standards (XDG, RFC 2119, SemVer)
- Minor spec clarifications

## [0.1.0 - draft] - 2026-02-28

### Added

- Initial public release of the TCA theme gallery
- TCA Specification v0.1.0
- JSON validation schema (`schema/tca.schema.pragmatic.json`)
- 10 themes: Catppuccin Mocha, Dracula, Everforest Dark, Gruvbox Dark, Mono, Nord Dark, One Dark, Rosé Pine, Solarized Light, Tokyo Night
- Contributing guide (`CONTRIBUTING.md`)
