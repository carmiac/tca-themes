# Contributing to TCA Themes

Thank you for your interest in contributing to the Terminal Colors Architecture theme collection!

## How to Contribute a Theme

### 1. Create Your Theme

Start with an existing theme as a template (e.g., `dracula.toml` or `nord-dark.toml`):

```toml
[theme]
name        = "My Amazing Theme"
slug        = "my-amazing-theme"
author      = "Your Name"
version     = "1.0.0"
description = "My favorite amazing theme."
dark        = true


[ansi]
# All ANSI values must be direct hex colors.
black   = "#000000"
red     = "#cc0000"
green   = "#00aa00"
yellow  = "#ccaa00"
blue    = "#001f3f"
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

[palette]
# Tones in each ramp should go from darkest to lightest (0-indexed arrays).
# The neutral ramp should cover the full lightness range.
neutral = ["#000000", "#1a1a1a", "#6a6a6a", "#bababa", "#ffffff"]

# Additional color ramps (optional).
# Use obvious color names: "blue" or "pink" is good; "my_colors" or "vibrant" is bad.
# Ramp values must be hex (#RRGGBB) or ansi.* references — no palette.* or base16.* references.
red    = ["#800000", "#cc0000", "#ff6666"]
yellow = ["#404000", "#ccaa00", "#ffe066"]
green  = ["#004000", "#00aa00", "#66ff66"]
cyan   = ["#004040", "#007499", "#66ddff"]
blue   = ["#001f3f", "#0074d9", "#7fdbff"]

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
highlight = "palette.cyan.1"
link      = "palette.blue.2"

[ui.bg]
primary   = "palette.neutral.0"
secondary = "palette.neutral.1"

[ui.fg]
primary   = "palette.neutral.4"
secondary = "palette.neutral.3"
muted     = "palette.neutral.2"

[ui.border]
primary = "palette.neutral.3"
muted   = "palette.neutral.2"

[ui.cursor]
primary = "palette.blue.1"
muted   = "palette.neutral.2"

[ui.selection]
bg = "palette.neutral.1"
fg = "palette.neutral.4"
```

### 2. Validate Your Theme

Before submitting, ensure your theme passes validation:

```bash
# Install the TCA CLI tool
cargo install tca

# Validate your theme
tca validate your-theme.toml
```

The validator checks:

- Schema compliance
- WCAG contrast ratios (for accessibility)
- Valid color references
- Required fields present

### 3. Test Your Theme

Test your theme with different applications:

```bash
# Export to your terminal format
tca export your-theme.toml kitty > ~/.config/kitty/theme.conf
tca export your-theme.toml alacritty > ~/.config/alacritty/colors.toml

# Test with Ratatui picker (if you have the Rust tools)
cd ../tca-rust/tca-ratatui
cargo run --example picker --features widgets,loader -- ../../tca-themes
```

### 4. Submit a Pull Request

1. Fork this repository
2. Add your theme file: `your-theme-name.toml`
3. Ensure your theme validates successfully
4. Submit a pull request with:
   - Clear title: "Add [Theme Name] theme"
   - Description of your theme's design philosophy
   - Screenshots (optional but appreciated!)

## Theme Guidelines

### Required Elements

All themes must include:

- Metadata (`name`)
- All 16 ANSI colors (direct hex values)

Themes are recommended to include:

- Extended metadata (`slug`, `author`, `version`, `description`, `dark`)
- A `palette` with a neutral ramp
- Base16 colors
- All 6 semantic colors
- All 11 UI fields

### Naming Conventions

- **File name**: `kebab-case.toml` (e.g., `nord-dark.toml`)
- **Theme name**: Human-readable (e.g., "Nord Dark")
- **Slug**: Same as filename without extension (e.g., "nord-dark")

### Color Rules

1. **ANSI values must be direct hex** (`#RRGGBB`)

2. **Palette ramps are 0-indexed arrays**
   - First element is index `0`
   - Reference as `palette.rampname.0`, `palette.rampname.1`, etc.

3. **Semantic and UI values may reference palette, ansi, or base16**
   - Base16 values may only reference palette or ansi (not other base16 entries)
   - Direct hex is allowed in all sections but palette references are preferred for maintainability

4. **ANSI bright keys use snake_case**: `bright_black`, `bright_red`, etc.

### Color Tips

1. **Neutral ramp**: Cover the full lightness range from darkest to lightest
2. **Contrast**: Ensure WCAG AA compliance (4.5:1 for normal text)
3. **Consistency**: Use palette references for semantic/ui to make adjustments easier
4. **Accessibility**: Test with different vision modes

## Questions?

- Open an issue for discussion
- Check existing themes for examples
- See the [TCA specification](SPECIFICATION.md) for full details

## Code of Conduct

Be respectful and constructive. We welcome themes of all styles that pass the validator.

## License

By contributing, you agree to release your file to the public domain.
