# Terminal Colors Architecture

TCA is a structured, portable format for terminal and cli/tui color themes. Define your palette once in TOML - export to any terminal, editor, or UI framework.

For Application Developers - Easily integrate color themes into your terminal application with library support for Python/Textual, Rust/Ratatui, and Go/Lipgloss support.

For Theme Developers - Create your theme your way with clear, simple guardrails to help your theme be used the right way.

For Theme Users - Install a theme file once, and then every TCA-aware application can use it. Export to Base16/24 for an expanded ecosystem of tools.

## Current State - DRAFT

This is currently a draft, but already supports most basic features including language libraries, theme validators, and import/export.

### Roadmap

- Expanded 'tca' tools for import and export, automatic theme downloads
- Per-user theme preference library support
- More themes
- Built in hardcoded themes for easy on-ramp

## Using These Themes

The primary `tca` tool is installable with:

```bash
cargo install tca-cli
```

Language support libraries are available at:

- [TCA-Rust](https://github.com/carmiac/tca-rust)
- [TCA-Python](https://github.com/carmiac/tca-python)
- [TCA-Go](https://github.com/carmiac/tca-go)

### Shared Theme Directory

TCA uses an XDG compliant shared directory for themes:

- Linux/BSD: $XDG_DATA_HOME/tca/themes (default: ~/.local/share/tca/themes)
- macOS: ~/Library/Application Support/tca/themes
- Windows: %APPDATA%\tca\themes

Themes in this directory can be loaded by name from anywhere.

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

# Many Others
tca export --help

```

### Preview with the Picker

```bash
cd ../tca-ratatui
cargo run --example picker --features widgets,loader -- ../themes
```

## Creating Your Own Themes

1. Start with one of these examples as a template
2. Modify the palette colors to your liking
3. Validate with `tca validate` to ensure schema compliance
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
