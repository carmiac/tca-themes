# Terminal Colors Architecture (TCA) Specification

**Version:** 0.4.0  
**Status:** Draft

## Overview

Terminal Colors Architecture (TCA) is a specification for defining terminal application color themes in a structured, portable format for use by developers and theme authors. TCA uses [Tinted Theming base24](https://github.com/tinted-theming/base24/) as the base format specification and adds a UI/TUI semantic layer definition, theme file location, user preference configuration and library validation specifications.

## Goals

1. **Portability** - Single theme file works across terminals, editors, and frameworks
2. **Interoperability** - Structured ANSI, Base16/24, semantic, and UI naming scheme allows for easy library integration
3. **Consistency** - Structured approach to semantic color naming
4. **Accessibility** - Built-in contrast validation and WCAG compliance

## File Format

TCA themes are base24 themes as defined by the [Tinted Theming Base24](https://github.com/tinted-theming/base24/) project. These files are a minimal subset of YAML.

**File naming convention:** `kebab-case.yaml` (e.g., `nord-dark.yaml`, `solarized-light.yaml`)

## File Location

In order to facilitate theme sharing between applications, per-user theme and preference files must be located in common directories. TCA respects the [XDG Directory Specification](https://specifications.freedesktop.org/basedir/latest/).

TCA themes must be installed into `$XDG_DATA_HOME/tca/themes/` (default: `~/.local/share/tca/themes/`).

## User Preferences File

User preferences must be stored in a TOML file at $XDG_CONFIG_HOME/tca/tca.toml with the following structure:

```toml
[tca]
default_theme = "User Default Theme"
default_dark_theme = "user-default-dark-theme"
default_light_theme = "userDefaultLightTheme"
```

Theme names can be in any standard case that can be folded into kebab-case.

All entries are optional. Fallback precedence should be:

1. Individual application preferences.
2. `default_dark_theme` if defined and terminal is in dark mode.
3. `default_light_theme` if defined and terminal is in light mode.
4. `default_theme` if the other preferences are not defined or terminal mode is unavailable.
5. Any defined theme if the others are not defined.

If no themes are defined or the user preference file is missing, any theme may be used.

### Platform Notes

Non-XDG compliant platforms (e.g. Windows) should follow platform conventions. Use of a standard cross-platform directory library is recommended.

---

## Semantic Mapping

TCA maps the base24 base names and metadata to a structured set of names for library and application code. Each section has a set of fields that may be referenced as `section.field` e.g. `meta.name` or `ui.fg_primary`. Libraries should implement access to sections and fields an idiomatic way for the language.

| Section  | Description                 |
| -------- | --------------------------- |
| meta     | Information about the theme |
| ansi     | 16 ANSI terminal colors     |
| semantic | Semantic colors             |
| ui       | UI element colors           |

### Metadata Fields

| Field    | Base24 Map | Description                       |
| -------- | ---------- | --------------------------------- |
| `name`   | `name`     | Human-readable theme name         |
| `author` | `author`   | Theme author name or organization |

### ANSI Fields

| Field          | Base24 Map |
| -------------- | ---------- |
| black          | base00     |
| red            | base08     |
| green          | base0b     |
| yellow         | base0a     |
| blue           | base0d     |
| magenta        | base0e     |
| cyan           | base0c     |
| white          | base05     |
| bright_black   | base03     |
| bright_red     | base12     |
| bright_green   | base14     |
| bright_yellow  | base13     |
| bright_blue    | base16     |
| bright_magenta | base17     |
| bright_cyan    | base15     |
| bright_white   | base07     |

### Semantic Section

| Field       | Base24 Map | Example Use               |
| ----------- | ---------- | ------------------------- |
| `error`     | base08     | Red text for errors       |
| `warning`   | base09     | Yellow/orange warnings    |
| `info`      | base0c     | Cyan info text            |
| `success`   | base0b     | Green checkmarks          |
| `highlight` | base0e     | Selection, search results |
| `link`      | base0d     | Clickable links           |

### UI Section

| Field            | Base24 Map | Example Use                    |
| ---------------- | ---------- | ------------------------------ |
| `fg.primary`     | base05     | Primary text color             |
| `fg.secondary`   | base06     | Secondary text color           |
| `fg.muted`       | base04     | Muted/dimmed text              |
| `bg.primary`     | base00     | Primary background color       |
| `bg.secondary`   | base01     | Secondary/alternate background |
| `border.primary` | base02     | Primary border color           |
| `border.muted`   | base01     | Subtle/muted borders           |
| `cursor.primary` | base05     | Active cursor color            |
| `cursor.muted`   | base04     | Inactive cursor color          |
| `selection.bg`   | base02     | Selection background           |
| `selection.fg`   | base05     | Selection foreground/text      |

---

## Library Conformance

A TCA library must satisfy the following requirements.

### Loading

1. A library MUST be able to load a base24 YAML theme file from a path.
2. A library MUST be able to load themes from the XDG theme directory.
3. A library MUST respect the user preference fallback chain defined in this spec.
4. A library SHOULD support loading themes by name in any common case format (snake_case, CamelCase, etc.) and fold them to kebab-case for lookup.
5. A library MAY provide built-in themes.

### Mapping

1. A library MUST expose all fields defined in the semantic mapping tables under their canonical names.
2. A library MUST expose colors in the native color type of its target framework or language.
3. A library MUST derive dark from the luminance of ui.bg.primary if not determinable from the environment.

### API

1. A library MUST provide access to mapped fields using idiomatic naming for its language (e.g. theme.semantic.error, theme.ui.bg_primary).
2. A library MUST provide a default theme that can be used without any theme files present.
3. A library SHOULD provide a builder or equivalent for constructing themes programmatically.
4. A library SHOULD expose base24 slot values directly for interoperability with non-TCA tooling.

### Validation

1. A library SHOULD warn when a loaded theme fails contrast recommendations.
2. A library MAY provide a standalone validation tool.

## Validation Rules

TCA themes must pass the following validation checks:

### 1. Structure Validation

- Valid base24 syntax

### 2. Contrast Validation (Recommended)

For accessibility the following minimum contrast ratios are recommended:

| Fields                      | Recommended | Warning | Error |
| --------------------------- | ----------- | ------- | ----- |
| fg.primary / bg.\*          | > 4.5       | < 3.5   | < 3.0 |
| fg.muted / bg.\*            | > 3.0       | < 2.5   | < 2.0 |
| semantic.\* / bg.\*         | > 4.5       | < 3.5   | < 3.0 |
| selection.fg / selection.bg | > 3.0       | < 2.5   | < 2.0 |
| border.\* / bg.\*           | > 3.0       | < 2.5   | < 2.0 |
| cursor.primary / bg.\*      | > 4.5       | < 3.5   | < 3.0 |
| cursor.muted / bg.\*        | > 3.0       | < 2.5   | < 2.0 |

---

## Design Principles

### 1. Prefer Semantic Names

Use semantic names (error, success) rather than base names (base00, base14) in application code.

### 2. Practical Accessibility Matters

Themes should meet [WCAG 2.0 AA](https://www.w3.org/WAI/standards-guidelines/wcag/) standards:

- 4.5:1 contrast ratio for normal text
- 3:1 for large text

However many popular existing themes do not, so the requirements are somewhat relaxed.

### 3. Framework Agnostic

TCA themes can be exported to any format supported by Tinted Theming:

- Terminal configs (Kitty, Alacritty, iTerm2)
- Editor themes (Vim, Helix, VS Code)
- UI frameworks (Ratatui, Textual, Lipgloss)
- Color schemes (Base16, tmux)
- etc...

---

## Tooling

A reference validation tool is provided, as are implementations for several languages and frameworks.

### Validation

```bash
tca validate theme.yaml
```

### Set Configuration

```bash
tca config set default "Tokyo Night"
```

### Add Themes to User Directory

```bash
tca add "Tokyo Night"
```

### CLI/TUI Frameworks

Reference implementations are provided for Rust, Go, and Python. Libraries are provided for integrating into the Ratatui, Lipgloss, and Textual frameworks.

---

## Standards Compliance

This specification follows the following standards:

- [Semantic Versioning 2.0](https://semver.org)
- [RFC 2119](https://soundcloud.com/ericwbailey/rfc-2119)
- [XDG Directory Specification](https://specifications.freedesktop.org/basedir/latest/)

## References

- [Tinted Theming](https://github.com/tinted-theming/home)
- [Base24](https://github.com/tinted-theming/base24/)
- [Contributing](CONTRIBUTING.md) - Guide for creating themes

---

## License

This specification is released to the public domain.
