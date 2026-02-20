# TCA Theme Gallery

Example themes demonstrating the Terminal Colors Architecture specification, and the validation schema.

## Using These Themes

### Validate a theme

```bash
cd ..
tca validate themes/nord-dark.toml
```

### Export to your terminal

```bash
# Kitty
tca export themes/dracula.toml -f kitty -o ~/.config/kitty/theme.conf

# Alacritty
tca export themes/nord-dark.toml -f alacritty -o ~/.config/alacritty/colors.toml

# tmux
tca export themes/solarized-light.toml -f tmux -o ~/.config/tmux/colors.conf
```

### Preview with the picker

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
