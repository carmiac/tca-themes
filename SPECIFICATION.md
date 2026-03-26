# Terminal Colors Architecture (TCA) Specification

**Version:** 0.3.0  
**Status:** Draft

## Overview

Terminal Colors Architecture (TCA) is a specification for defining terminal application color themes in a structured, portable format for use by developers and theme authors. TCA themes are defined in TOML and may be used across multiple terminal applications, UI frameworks, and color schemes.

## Goals

1. **Portability** - Single theme file works across terminals, editors, and frameworks
2. **Interoperability** - Structured ANSI, Base16/24, semantic, and UI naming scheme allows for easy library integration
3. **Consistency** - Structured approach to semantic color naming
4. **Accessibility** - Built-in contrast validation and WCAG compliance
5. **Flexibility** - Supports both simple and complex color schemes

## File Format

TCA themes are defined in TOML (`.toml` extension).

**File naming convention:** `kebab-case.toml` (e.g., `nord-dark.toml`, `solarized-light.toml`)

## Structure

A TCA theme consists of six sections:

```toml
[theme]    # Theme metadata          (required)
[ansi]     # 16 ANSI terminal colors (required)
[palette]  # Color ramps             (optional)
[base16]   # Base16/24 compatibility (optional)
[semantic] # Semantic color names    (required)
[ui]       # UI element colors       (required)
```

Sections must appear in this order in the file. Each section may reference colors defined in any earlier section.

---

## 1. Theme Metadata Section

**Required**

All themes must include a metadata section for identification and attribution.

### Fields

| Field         | Type   | Required | Description                         |
| ------------- | ------ | -------- | ----------------------------------- |
| `name`        | string | Yes      | Human-readable theme name           |
| `author`      | string | No       | Theme author name or organization   |
| `version`     | string | No       | Theme version (semver recommended)  |
| `description` | string | No       | Brief description of the theme      |
| `dark`        | bool   | No       | True if the theme a dark mode theme |

If the `dark` field is absent libraries should determine if a theme is dark or light using the ui.bg.primary / ui.fg.primary luminance ratio.

### Example

```toml
[theme]
name = "Nord Dark"
author = "Arctic Ice Studio"
version = "1.0.0"
description = "An arctic, north-bluish color palette"
dark = true
```

---

## 2. ANSI Section

**Required**

All themes must define the 16 standard ANSI terminal colors.

### Fields

All fields are **required**.

**Normal Colors:**

- `black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, `white`

**Bright Colors:**

- `bright_black`, `bright_red`, `bright_green`, `bright_yellow`
- `bright_blue`, `bright_magenta`, `bright_cyan`, `bright_white`

### Value Format

- Must be hex colors in format `#RRGGBB` (6-digit hex)

### Example

```toml
[ansi]
black   = "#000000"
red     = "#800000"
green   = "#008000"
yellow  = "#808000"
blue    = "#000080"
magenta = "#800080"
cyan    = "#008080"
white   = "#c0c0c0"
bright_black   = "#808080"
bright_red     = "#ff0000"
bright_green   = "#00ff00"
bright_yellow  = "#ffff00"
bright_blue    = "#0000ff"
bright_magenta = "#ff00ff"
bright_cyan    = "#00ffff"
bright_white   = "#ffffff"
```

## 3. Palette Section

**Optional**

The palette is used to define other colors that may be used elsewhere in the theme using color ramps. Colors in the palette may be defined by either hex strings or references to colors from the ANSI section. Color ramp entries must be single hue lightness ramps that scale from darkest to lightest.

### Structure

```toml
[palette]
gray = [           # Greyscale-ish ramp
  "ansi.black",
  "#4a4a4a",
  "#8a8a8a",
  "ansi.white",
  "ansi.bright_white",
]

red = ["#3b1e1e", "#bf616a", "#d08770"]
```

### Rules

1. Ramp Values:
   - Must be hex colors in format `#RRGGBB` (6-digit hex) or ansi colors in format `ansi.color`

2. Ramp Naming:
   - Use descriptive color names: `red`, `blue`, `green`, `purple`
   - Avoid generic names: `vibrant`, `accent`, `custom`

3. Ramp Ordering:
   - Within each ramp, earlier colors must be darker
   - Higher indexes must be lighter

4. Ramp Indexing:
   - Arrays are 0-indexed: the first element is index `0`
   - Example: `palette.red.0` refers to the first element of the `red` ramp

---

## 4. Base16/24 Section

**Recommended**

Base16/24 color mapping for compatibility with Base16 color ecosystem.

### Fields

16 or 24 fields from `base00` through `base17`, all optional.

### Value Format

May be a direct hex string, or a reference to ANSI or palette colors.

### Example

```toml
[base16]
base00 = "palette.neutral.0"
base01 = "palette.neutral.1"
base02 = "palette.neutral.1"
base03 = "palette.neutral.2"
base04 = "palette.neutral.2"
base05 = "palette.neutral.3"
base06 = "palette.neutral.4"
base07 = "palette.neutral.4"
base08 = "ansi.red"
base09 = "#ffa500"
base0A = "ansi.yellow"
base0B = "ansi.green"
base0C = "ansi.cyan"
base0D = "ansi.blue"
base0E = "ansi.magenta"
base0F = "ansi.bright_red"
```

Implementations may expose semantic aliases for Base16 slots (e.g. keyword for base0E, string for base0B) as a library convenience. These aliases are not part of the TCA theme format and should not appear in theme files.

---

## 5. Semantic Section

Required

Semantic colors provide meaningful color names for common use cases.

### Standard Fields

| Field       | Purpose                | Example Use               |
| ----------- | ---------------------- | ------------------------- |
| `error`     | Error messages         | Red text for errors       |
| `warning`   | Warning messages       | Yellow/orange warnings    |
| `info`      | Informational messages | Blue info text            |
| `success`   | Success messages       | Green checkmarks          |
| `highlight` | Highlighted text       | Selection, search results |
| `link`      | Hyperlinks             | Clickable links           |

### Value Format

May be a direct hex string, or a reference to ANSI, base16, or palette colors.

### Example

```toml
[semantic]
error     = "ansi.bright_red"
warning   = "ansi.yellow"
info      = "ansi.blue"
success   = "ansi.green"
highlight = "base16.base03"
link      = "#2000FF"
```

---

## 6. UI Section

Required

UI colors define colors for user interface elements.

### Standard Fields

**Background:**

- `bg.primary` - Primary background color
- `bg.secondary` - Secondary/alternate background

**Foreground:**

- `fg.primary` - Primary text color
- `fg.secondary` - Secondary text color
- `fg.muted` - Muted/dimmed text

**Borders:**

- `border.primary` - Primary border color
- `border.muted` - Subtle/muted borders

**Cursor:**

- `cursor.primary` - Active cursor color
- `cursor.muted` - Inactive cursor color

**Selection:**

- `selection.bg` - Selection background
- `selection.fg` - Selection foreground/text

In TOML, the dotted field names map to nested sub-tables. For example, `bg.primary` is written as `primary` under `[ui.bg]`.

### Value Format

May be a direct hex string, or a reference to ansi, base16, or palette colors.

### Example

```toml
[ui.bg]
primary   = "base16.base00"
secondary = "base16.base01"

[ui.fg]
primary   = "base16.base05"
secondary = "base16.base04"
muted     = "ansi.white"

[ui.border]
primary = "ansi.bright_white"
muted   = "ansi.white"

[ui.cursor]
primary = "ansi.yellow"
muted   = "palette.gray.2"

[ui.selection]
bg = "base16.base02"
fg = "ansi.bright_white"
```

---

## File Location and User Configuration

In order to facilitate theme sharing between applications, per-user theme and preference files must be located in common directories. TCA respects the [XDG Directory Specification](https://specifications.freedesktop.org/basedir/latest/).

### Theme File Location

TCA themes must be installed into `$XDG_DATA_HOME/tca/themes/` (default: `~/.local/share/tca/themes/`).

### User Preferences File

User preferences must be stored in a TOML file at $XDG_CONFIG_HOME/tca/tca.toml with the following structure:

```toml
[tca]
default_theme = "user-default-theme"
default_dark_theme = "user-default-dark-theme"
default_light_theme = "user-default-light-theme"
```

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

## Validation Rules

TCA themes must pass the following validation checks:

### 1. Structure Validation

- Valid TOML syntax
- Required sections present (theme, ANSI, ui, semantic)
- All required fields present

### 2. Reference Validation

- No references in ANSI section
- References only refer to prior sections
- All referenced colors exist

### 3. Contrast Validation (Recommended)

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

## Complete Example

```toml
[theme]
name = "Example Theme"
author = "Theme Author"
version = "1.0.0"
description = "An example TCA theme"
dark = true

[palette]
neutral = ["#1a1a1a", "#333333", "#666666", "#999999", "#f0f0f0"]
red     = ["#3d0000", "#cc0000", "#ff6666"]
green   = ["#003d00", "#00aa00", "#66ff66"]
blue    = ["#001f3f", "#0074d9", "#7fdbff"]
yellow  = ["#3d3d00", "#ccaa00", "#ffe066"]

[ansi]
black   = "#1a1a1a"
red     = "#cc0000"
green   = "#00aa00"
yellow  = "#ccaa00"
blue    = "#0074d9"
magenta = "#8800aa"
cyan    = "#007499"
white   = "#999999"

bright_black   = "#333333"
bright_red     = "#ff6666"
bright_green   = "#66ff66"
bright_yellow  = "#ffe066"
bright_blue    = "#7fdbff"
bright_magenta = "#cc66ff"
bright_cyan    = "#66ddff"
bright_white   = "#f0f0f0"

[base16]
base00 = "palette.neutral.0"
base01 = "palette.neutral.1"
base02 = "palette.neutral.1"
base03 = "palette.neutral.2"
base04 = "palette.neutral.2"
base05 = "palette.neutral.3"
base06 = "palette.neutral.4"
base07 = "palette.neutral.4"
base08 = "palette.red.1"
base09 = "#ff8800"
base0A = "palette.yellow.1"
base0B = "palette.green.1"
base0C = "ansi.cyan"
base0D = "palette.blue.1"
base0E = "ansi.magenta"
base0F = "palette.red.0"

[semantic]
error     = "palette.red.1"
warning   = "palette.yellow.1"
info      = "palette.blue.1"
success   = "palette.green.1"
highlight = "ansi.bright_yellow"
link      = "palette.blue.2"

[ui.bg]
primary   = "palette.neutral.0"
secondary = "palette.neutral.1"

[ui.fg]
primary   = "palette.neutral.4"
secondary = "palette.neutral.3"
muted     = "palette.neutral.2"

[ui.border]
primary = "palette.neutral.2"
muted   = "palette.neutral.1"

[ui.cursor]
primary = "palette.blue.1"
muted   = "palette.neutral.3"

[ui.selection]
bg = "palette.neutral.1"
fg = "palette.neutral.4"
```

---

## Design Principles

### 1. Semantic over Literal

Use semantic names (error, success) rather than literal names (red, green) in application code. This allows theme authors flexibility in color choices.

### 2. Practical Accessibility Matters

Themes should meet [WCAG 2.0 AA](https://www.w3.org/WAI/standards-guidelines/wcag/) standards:

- 4.5:1 contrast ratio for normal text
- 3:1 for large text

However many popular existing themes do not, so the requirements are somewhat relaxed.

### 3. Framework Agnostic

TCA themes can be exported to any format:

- Terminal configs (Kitty, Alacritty, iTerm2)
- Editor themes (Vim, Helix, VS Code)
- UI frameworks (Ratatui, Textual, Lipgloss)
- Color schemes (Base16, tmux)

---

## Tooling

A reference validation and export tool is provided, as are implementations for several languages and frameworks.

### Validation

```bash
tca validate theme.toml
```

### Export

```bash
tca export theme.toml kitty
tca export theme.toml alacritty
tca export theme.toml vim
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

- [Examples](themes/) - Example themes demonstrating the specification
- [Contributing](CONTRIBUTING.md) - Guide for creating themes

---

## License

This specification is released to the public domain.
