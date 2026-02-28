# TCA Theme Gallery

Example themes demonstrating the Terminal Colors Architecture specification, and the validation schema.

## Themes

| Theme | Mode | Description |
| --- | --- | --- |
| [Catppuccin Mocha](themes/catppuccin-mocha.toml) | Dark | Soothing pastel dark theme |
| [Dracula](themes/dracula.toml) | Dark | Classic vampire-inspired dark theme |
| [Everforest Dark](themes/everforest-dark.toml) | Dark | Comfortable green-tinted dark theme with warm, natural tones |
| [Gruvbox Dark](themes/gruvbox-dark.toml) | Dark | Retro groove dark theme with warm, earthy tones |
| [Mono](themes/mono.toml) | Dark | Minimal monochromatic greyscale theme |
| [Nord Dark](themes/nord-dark.toml) | Dark | Arctic, north-bluish color palette |
| [One Dark](themes/one-dark.toml) | Dark | Clean dark theme inspired by Atom's One Dark UI |
| [Rosé Pine](themes/rose-pine.toml) | Dark | Natural pine, faux fur, and soho vibes |
| [Solarized Light](themes/solarized-light.toml) | Light | Precision colors for machines and people |
| [Tokyo Night](themes/tokyo-night.toml) | Dark | VSCode-inspired dark theme, city lights of Tokyo |

## Using These Themes

### Validate a Theme

```bash
tca validate themes/nord-dark.toml
```

### Export to Your Terminal

```bash
# Kitty
tca export themes/dracula.toml -f kitty -o ~/.config/kitty/theme.conf

# Alacritty
tca export themes/nord-dark.toml -f alacritty -o ~/.config/alacritty/colors.toml

# tmux
tca export themes/solarized-light.toml -f tmux -o ~/.config/tmux/colors.conf
```

### Preview with the Picker

```bash
cd ../tca-ratatui
cargo run --example picker --features widgets,loader -- ../themes
```

## Creating Your Own Themes

1. Start with one of these examples as a template
2. Modify the palette colors to your liking
3. Validate with `tca-validator` to ensure schema compliance
4. Test exports to your preferred terminal/editor
5. Share with the community!

## Validation Status

All themes in this gallery:

- Pass schema validation
- Meet WCAG contrast requirements
- Export successfully to all supported formats
- Work with the Ratatui theme picker

## Contributing Themes

Have a great theme? Contributions welcome! Ensure your theme:

- Follows the TCA specification
- Passes validator checks
- Includes metadata (name, author, version)
- Documents its design philosophy
