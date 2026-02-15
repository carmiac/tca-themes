# TCA Theme Gallery

Example themes demonstrating the Terminal Colors Architecture specification.

## Themes

### Nord Dark
**File:** `nord-dark.yaml`

A cool, arctic-inspired dark theme with muted colors and excellent readability. Features a neutral gray palette with frost (blue) and aurora (colorful accent) ramps.

- **Style:** Cool, minimal, professional
- **Best for:** Long coding sessions, low-light environments
- **Palette highlights:** Arctic frost blues, aurora accents

### Dracula
**File:** `dracula.yaml`

A vibrant dark theme with high-contrast, saturated colors. Perfect for users who want colorful, energetic aesthetics without sacrificing readability.

- **Style:** Vibrant, energetic, high-contrast
- **Best for:** Modern applications, creative work
- **Palette highlights:** Saturated purples, pinks, cyans

### Solarized Light
**File:** `solarized-light.yaml`

A precision-tuned light theme with carefully selected contrast ratios. Based on the popular Solarized color scheme with warm tones and excellent accessibility.

- **Style:** Warm, balanced, accessible
- **Best for:** Bright environments, daylight work
- **Palette highlights:** Warm beiges, balanced accent colors

### Mono
**File:** `mono.yaml`

A minimal, monochrome theme using only neutral grays. Demonstrates that TCA works even with extreme simplicity.

- **Style:** Minimal, distraction-free, elegant
- **Best for:** Focus work, writing, minimalists
- **Palette highlights:** Pure grayscale, no color distractions

## Using These Themes

### Validate a theme
```bash
cd ..
tca-validator/target/release/tca-validator themes/nord-dark.yaml
```

### Export to your terminal
```bash
# Kitty
tca-export/target/release/tca-export themes/dracula.yaml -f kitty -o ~/.config/kitty/theme.conf

# Alacritty
tca-export/target/release/tca-export themes/nord-dark.yaml -f alacritty -o ~/.config/alacritty/colors.toml

# tmux
tca-export/target/release/tca-export themes/solarized-light.yaml -f tmux -o ~/.config/tmux/colors.conf
```

### Preview with the picker
```bash
cd ../tca-ratatui
cargo run --example picker --features widgets,loader -- ../themes
```

## Creating Your Own Themes

1. Start with one of these examples as a template
2. Modify the palette colors to your liking
3. Validate with `tca-validator` to ensure WCAG contrast compliance
4. Test exports to your preferred terminal/editor
5. Share with the community!

## Validation Status

All themes in this gallery:
- ✅ Pass schema validation
- ✅ Meet WCAG contrast requirements
- ✅ Export successfully to all supported formats
- ✅ Work with the Ratatui theme picker

## Contributing Themes

Have a great theme? Contributions welcome! Ensure your theme:
- Follows the TCA specification
- Passes validator checks
- Includes metadata (name, author, version)
- Documents its design philosophy
