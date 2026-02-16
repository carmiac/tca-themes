# Terminal Colors Architecture (TCA) Specification

**Version:** 1.0.0  
**Status:** Draft  
**Last Updated:** 2026-02-15

## Overview

Terminal Colors Architecture (TCA) is a specification for defining terminal application color themes in a structured, portable format for use by developers and theme authors. TCA themes are defined in YAML and can be used across multiple terminal applications, UI frameworks, and color schemes.

## Goals

1. **Portability** - Single theme file works across terminals, editors, and frameworks
2. **Interoperability** - Structured ANSI, Base16, semantic, and UI naming scheme allows for easy library integration
3. **Consistency** - Structured approach to semantic color naming
4. **Accessibility** - Built-in contrast validation and WCAG compliance
5. **Flexibility** - Supports both simple and complex color schemes

## File Format

TCA themes are defined in YAML (`.yaml` or `.yml` extension).

**File naming convention:** `kebab-case.yaml` (e.g., `nord-dark.yaml`, `solarized-light.yaml`)

## Structure

A TCA theme consists of five sections:

```yaml
meta: # Theme metadata (required)
palette: # Color ramps (required)
ansi: # 16 ANSI terminal colors (required)
semantic: # Semantic color names (optional)
ui: # UI element colors (optional)
base16: # Base16 compatibility (optional)
```

---

## 1. Meta Section

**Status:** Required

Theme metadata for identification and attribution.

### Fields

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | Human-readable theme name |
| `slug` | string | No | URL-safe identifier (should match filename) |
| `author` | string | No | Theme author name or organization |
| `version` | string | No | Theme version (semver recommended) |
| `description` | string | No | Brief description of the theme |

### Example

```yaml
meta:
  name: "Nord Dark"
  slug: "nord-dark"
  author: "Arctic Ice Studio"
  version: "1.0.0"
  description: "An arctic, north-bluish color palette"
```

---

## 2. Palette Section

**Status:** Required

The palette defines the foundational colors of the theme using color ramps. All other sections reference colors from the palette.

### Structure

```yaml
palette:
  neutral: # Required neutral (grayscale-ish) ramp
    "1": "#000000"
    "2": "#4a4a4a"
    "3": "#8a8a8a"
    "4": "#c0c0c0"
    "5": "#ffffff"

  <rampname>: # Optional additional ramps (red, blue, etc.)
    "1": "#darkcolor"
    "5": "#lightcolor"
```

### Rules

1. **Required Ramp:**
   - Must have a `neutral` ramp for grayscale-ish colors
   - `neutral` ramp must have all five keys.

2. **Ramp Keys:**
   - Keys must be strings from "1" to "5"
   - Valid: `"1"`, `"2"`, `"3"`, `"4"`, `"5"`
   - Invalid: `"0"`, `"6"`, `"10"`, `"000"`, `"900"`
   - Additional ramps can be sparse (don't need all 5 keys)
   - Example: `{"1": "#0f00f1f", "3": "#6a6a8c", "5": "#f0f0ff"}` is valid

3. **Ramp Values:**
   - Must be hex colors in format `#RRGGBB` (6-digit hex)
   - Invalid: `#RGB` (3-digit), `rgb(...)`, color names

4. **Ramp Naming:**
   - Use descriptive color names: `red`, `blue`, `green`, `purple`
   - Avoid generic names: `vibrant`, `accent`, `custom`

5. **Ramp Ordering:**
   - Within each ramp, lower numbers should be darker
   - Higher numbers should be lighter
   - Example: `"1"` = darkest, `"5"` = lightest

### Example

```yaml
palette:
  neutral:
    "1": "#2e3440" # Darkest gray
    "2": "#3b4252"
    "3": "#434c5e"
    "4": "#4c566a"
    "5": "#eceff4" # Lightest gray

  red:
    "1": "#3b1e1e"
    "3": "#bf616a"
    "5": "#d08770"

  blue:
    "2": "#5e81ac" # Sparse - no "1"
    "4": "#81a1c1" # Sparse - no "3" or "5"
```

---

## 3. ANSI Section

**Status:** Required

Defines the 16 standard ANSI terminal colors.

### Fields

All fields are **required**.

**Normal Colors:**

- `black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, `white`

**Bright Colors:**

- `brightBlack`, `brightRed`, `brightGreen`, `brightYellow`
- `brightBlue`, `brightMagenta`, `brightCyan`, `brightWhite`

### Value Format

Values must be palette references in the format:

```
"palette.<rampname>.<key>"
```

- Valid: `"palette.neutral.1"`, `"palette.red.3"`
- Invalid: `"#ff0000"` (direct hex)
- Invalid: `"$palette.neutral.1"` ($ prefix)
- Invalid: `"$neutral.1"` (shorthand)

### Example

```yaml
ansi:
  black: "palette.neutral.1"
  red: "palette.red.3"
  green: "palette.green.3"
  yellow: "palette.yellow.3"
  blue: "palette.blue.3"
  magenta: "palette.purple.3"
  cyan: "palette.cyan.3"
  white: "palette.neutral.5"

  brightBlack: "palette.neutral.2"
  brightRed: "palette.red.4"
  brightGreen: "palette.green.4"
  brightYellow: "palette.yellow.4"
  brightBlue: "palette.blue.4"
  brightMagenta: "palette.purple.4"
  brightCyan: "palette.cyan.4"
  brightWhite: "palette.neutral.5"
```

---

## 4. Semantic Section

Optional (recommended)

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

All fields are optional, but recommended for consistency.

### Value Format

Same as ANSI section - must use palette references:

```
"palette.<rampname>.<key>"
```

### Example

```yaml
semantic:
  error: "palette.red.4"
  warning: "palette.yellow.4"
  info: "palette.blue.4"
  success: "palette.green.4"
  highlight: "palette.cyan.4"
  link: "palette.blue.5"
```

---

## 5. UI Section

Optional (recommended)

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

All fields are optional.

### Value Format

Same as other sections - must use palette references:

```
"palette.<rampname>.<key>"
```

### Example

```yaml
ui:
  bg.primary: "palette.neutral.1"
  bg.secondary: "palette.neutral.2"
  fg.primary: "palette.neutral.5"
  fg.secondary: "palette.neutral.4"
  fg.muted: "palette.neutral.3"
  border.primary: "palette.neutral.3"
  border.muted: "palette.neutral.2"
  cursor.primary: "palette.blue.4"
  cursor.muted: "palette.neutral.4"
  selection.bg: "palette.neutral.2"
  selection.fg: "palette.neutral.5"
```

---

## 6. Base16 Section

Optional

Base16 color mapping for compatibility with Base16 themes.

### Fields

16 fields from `base00` through `base0F`, all optional.

### Value Format

Same as other sections - must use palette references.

### Example

```yaml
base16:
  base00: "palette.neutral.1"
  base01: "palette.neutral.2"
  base02: "palette.neutral.2"
  base03: "palette.neutral.3"
  base04: "palette.neutral.4"
  base05: "palette.neutral.5"
  base06: "palette.neutral.5"
  base07: "palette.neutral.5"
  base08: "palette.red.3"
  base09: "palette.orange.3"
  base0A: "palette.yellow.3"
  base0B: "palette.green.3"
  base0C: "palette.cyan.3"
  base0D: "palette.blue.3"
  base0E: "palette.purple.3"
  base0F: "palette.red.4"
```

---

## Validation Rules

TCA themes must pass the following validation checks:

### 1. Structure Validation

- Valid YAML syntax
- Required sections present (meta, palette, ansi)
- All required fields present

### 2. Palette Validation

- Neutral ramp exists and contains keys 1-5
- All ramp keys are in range 1-5
- All palette values are valid hex colors (#RRGGBB)

### 3. Reference Validation

- No direct hex colors in ansi/semantic/ui/base16
- All palette references use correct format
- All referenced palette colors exist

### 4. Contrast Validation (Recommended)

- WCAG AA contrast ratios (4.5:1 for normal text)
- Neutral ramp luminance span >= 0.40
- Adjacent neutral steps >= 0.01 luminance difference

Failures in sections 1-3 are **errors** (theme is invalid).  
Failures in section 4 are **warnings** (theme is valid but may have accessibility issues).

---

## Complete Example

```yaml
meta:
  name: "Example Theme"
  slug: "example-theme"
  author: "Theme Author"
  version: "1.0.0"
  description: "An example TCA theme"

palette:
  neutral:
    "1": "#1a1a1a"
    "2": "#333333"
    "3": "#666666"
    "4": "#999999"
    "5": "#f0f0f0"

  red:
    "1": "#3d0000"
    "3": "#cc0000"
    "5": "#ff6666"

  blue:
    "1": "#001f3f"
    "3": "#0074d9"
    "5": "#7fdbff"

ansi:
  black: "palette.neutral.1"
  red: "palette.red.3"
  green: "palette.green.3"
  yellow: "palette.yellow.3"
  blue: "palette.blue.3"
  magenta: "palette.purple.3"
  cyan: "palette.cyan.3"
  white: "palette.neutral.5"
  brightBlack: "palette.neutral.2"
  brightRed: "palette.red.4"
  brightGreen: "palette.green.4"
  brightYellow: "palette.yellow.4"
  brightBlue: "palette.blue.4"
  brightMagenta: "palette.purple.4"
  brightCyan: "palette.cyan.4"
  brightWhite: "palette.neutral.5"

semantic:
  error: "palette.red.4"
  warning: "palette.yellow.4"
  info: "palette.blue.4"
  success: "palette.green.4"
  highlight: "palette.cyan.4"
  link: "palette.blue.5"

ui:
  bg.primary: "palette.neutral.1"
  bg.secondary: "palette.neutral.2"
  fg.primary: "palette.neutral.5"
  fg.secondary: "palette.neutral.4"
  fg.muted: "palette.neutral.3"
  border.primary: "palette.neutral.3"
  border.muted: "palette.neutral.2"
  cursor.primary: "palette.blue.4"
  cursor.muted: "palette.neutral.4"
  selection.bg: "palette.neutral.2"
  selection.fg: "palette.neutral.5"
```

---

## Design Principles

### 1. Single Source of Truth

All colors are defined in the palette. Other sections reference the palette. This ensures:

- Consistency across the theme
- Easy color adjustments
- Clear color relationships

### 2. Semantic Over Literal

Use semantic names (error, success) rather than literal names (red, green) in application code. This allows theme authors flexibility in color choices.

### 3. Accessibility Matters

Themes should meet WCAG AA standards:

- 4.5:1 contrast ratio for normal text
- 3:1 for large text
- Sufficient luminance range in neutral ramp

### 4. Framework Agnostic

TCA themes can be exported to any format:

- Terminal configs (Kitty, Alacritty, iTerm2)
- Editor themes (Vim, Helix, VS Code)
- UI frameworks (Ratatui, Textual, Lipgloss)
- Color schemes (Base16, tmux)

---

## Tooling

### Validation

```bash
tca validate theme.yaml
```

### Export

```bash
tca export theme.yaml kitty
tca export theme.yaml alacritty
tca export theme.yaml vim
```

### Loading

Themes can be loaded from:

1. Explicit file path
2. Shared themes directory (`$XDG_DATA_HOME/tca/themes`)
3. Current directory

### CLI/TUI Frameworks

Reference implementations are provided for Rust, Go, and Python. Libraries are provided for integrating into the Ratatui, Lipgloss, and Textual frameworks.

---

## Versioning

This specification follows semantic versioning:

- **Major version** - Breaking changes to theme format
- **Minor version** - New optional fields or features
- **Patch version** - Clarifications and fixes

---

## References

- [Schema](schema/tca.schema.pragmatic.json) - JSON Schema for validation
- [Examples](themes/) - Example themes demonstrating the specification
- [Contributing](CONTRIBUTING.md) - Guide for creating themes

---

## License

This specification is released to the public domain.
