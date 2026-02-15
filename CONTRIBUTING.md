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
  # Color ramps can have up to 5 colors, and can be sparse.
  # Tones in each ramp should go from darkest to lightest.
  neutral:
    "1": "#000000"
    "2": "#1a1a1a"
    # ... your neutral ramp, must include 1-5
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
  black: "palette.neutral.0"
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
  bg.primary: "$neutral.0"
  bg.secondary: "$neutral.1"
  fg.primary: "$neutral.5"
  fg.secondary: "$neutral.4"
  fg.muted: "$neutral.5"
  border.primary: "$neutral.4"
  border.muted: "$neutral.3"
  cursor.primary: "$blue.5"
  cursor.muted: "$neutral.3"
  selection.bg: "$neutral.2"
  selection.fg: "$neutral.5"
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

### Color Palette Tips

1. **Neutral ramp**: Use 5 shades (1-5) from darkest to lightest
2. **Contrast**: Ensure WCAG AA compliance (4.5:1 for normal text)
3. **Consistency**: Use palette references (`$neutral.5`) for maintainability
4. **Accessibility**: Test with different vision modes

## Questions?

- Open an issue for discussion
- Check existing themes for examples
- See the [TCA specification](schema.json) for full details

## Code of Conduct

Be respectful and constructive. We welcome themes of all styles that pass the validator. Nazis can fuck off though.

## License

By contributing, you agree to release your file to the public domain.
