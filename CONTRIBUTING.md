# Contributing to TCA Themes

Thank you for your interest in contributing to the Terminal Colors Architecture theme collection!

## How to Contribute a Theme

### 1. Create Your Theme

Start with an existing theme as a template (e.g., `dracula.yaml` or `nord-dark.yaml`):

```yaml
meta:
  name: "My Amazing Theme"
  slug: "my-amazing-theme"
  author: "Your Name"
  version: "1.0.0"
  description: "My favorite amazing theme."

palette:
  # Tones in each ramp should go from darkest to lightest.
  neutral:
    # The neutral ramp is required and must include all 5 tones.
    "1": "#000000"
    "2": "#6a6a6a"
    "3": "#1a1a1a"
    "4": "#bababa"
    "5": "#ffffff"

  # Additional color ramps (optional)
  # Should use obvious names for colors.
  # i.e. "blue" or "pink" is good
  #      "my_colors" or "vibrant" is bad.
  # Numbering can be sparse.
  blue:
    "1": "#001f3f"
    "5": "#7fdbff"

ansi:
  black: "palette.neutral.1"
  # ... your blue ramp
  blue: "palette.blue.5"
  # ... all 16 ANSI colors

semantic:
  error: "palette.red.5"
  warning: "palette.yellow.5"
  info: "palette.blue.5"
  success: "palette.green.5"
  highlight: "palette.cyan.5"
  link: "palette.blue.5"

ui:
  bg.primary: "palette.neutral.1"
  bg.secondary: "palette.neutral.2"
  fg.primary: "palette.neutral.5"
  fg.secondary: "palette.neutral.4"
  fg.muted: "palette.neutral.5"
  border.primary: "palette.neutral.4"
  border.muted: "palette.neutral.3"
  cursor.primary: "palette.blue.5"
  cursor.muted: "palette.neutral.3"
  selection.bg: "palette.neutral.2"
  selection.fg: "palette.neutral.5"
```

### 2. Validate Your Theme

Before submitting, ensure your theme passes validation:

```bash
# Install the TCA CLI tool
cargo install tca

# Validate your theme
tca validate your-theme.yaml
```

The validator checks:

- Schema compliance
- Palette ramp keys are 1-5 only
- No direct hex values in ansi/semantic/ui/base16
- WCAG contrast ratios (for accessibility)
- Valid color references
- Required fields present

### 3. Test Your Theme

Test your theme with different applications:

```bash
# Export to your terminal format
tca export your-theme.yaml kitty > ~/.config/kitty/theme.conf
tca export your-theme.yaml alacritty > ~/.config/alacritty/colors.toml

# Test with Ratatui picker (if you have the Rust tools)
cd ../tca-rust/tca-ratatui
cargo run --example picker --features widgets,loader -- ../../tca-themes
```

### 4. Submit a Pull Request

1. Fork this repository
2. Add your theme file: `your-theme-name.yaml`
3. Ensure your theme validates successfully
4. Submit a pull request with:
   - Clear title: "Add [Theme Name] theme"
   - Description of your theme's design philosophy
   - Screenshots (optional but appreciated!)

## Theme Guidelines

### Required Elements

All themes must include:

- Complete metadata (name, slug, author, version)
- Neutral palette ramp (1-5)
- All 16 ANSI colors

Themes are recommended to include:

- Base16 colors
- All 6 semantic colors
- All 11 UI fields
- Extended metadata (description, url, license)

### Naming Conventions

- **File name**: `kebab-case.yaml` (e.g., `nord-dark.yaml`)
- **Theme name**: Human-readable (e.g., "Nord Dark")
- **Slug**: Same as filename without extension (e.g., "nord-dark")

### Color Palette Rules

**IMPORTANT - These rules are strictly enforced:**

1. **Ramp Keys Must Be 1-5**
   - Valid: `"1"`, `"2"`, `"3"`, `"4"`, `"5"`
   - Invalid: `"0"`, `"6"`, `"10"`, `"000"`-`"900"`
   - Sparse ramps are allowed (e.g., just `"1"`, `"3"`, `"5"`)

2. **Direct Hex ONLY in Palette Section**
   - good: `palette.neutral.1: "#000000"`
   - bad: `ansi.black: "#000000"` (must use `"palette.neutral.1"`)

3. **All ansi/semantic/ui/base16 Must Reference Palette**
   - Cannot use direct hex colors
   - Must use format: `"palette.rampname.key"`

### Color Palette Tips

1. **Neutral ramp**: Use 5 shades (1-5) from darkest to lightest, but sparse is OK
2. **Contrast**: Ensure WCAG AA compliance (4.5:1 for normal text)
3. **Consistency**: Use palette references (`palette.neutral.3`) for maintainability
4. **Accessibility**: Test with different vision modes

## Questions?

- Open an issue for discussion
- Check existing themes for examples
- See the [TCA specification](schema.json) for full details

## Code of Conduct

Be respectful and constructive. We welcome themes of all styles that pass the validator. Nazis can fuck off though.

## License

By contributing, you agree to release your file to the public domain.
