# Terminal Colors Architecture

TCA is a structured, portable format for terminal and cli/tui color themes. Define your theme once as a base24 YAML file — use it everywhere.

For Application Developers: Easily integrate color themes into your terminal application with library support for Python/Textual, Rust/Ratatui, and Go/Lipgloss support.

For Theme Developers: Create your theme your way with clear, simple guardrails to help your theme be used the right way.

For Theme Users: Install a theme file once, and then every TCA-aware application can use it. Export to Base16/24 for an expanded ecosystem of tools.

## Current State - DRAFT

This is currently a draft in active development.

## Specification

The full specification is in [`SPECIFICATION.md`](SPECIFICATION.md). It defines the semantic mapping from base24 slots to named colors, library conformance requirements, and file/config locations.

## Using TCA

The primary `tca` tool is installable with:

```bash
cargo install tca-cli
```

Language support libraries are available at:

- [TCA-Rust](https://github.com/carmiac/tca-rust)
- [TCA-Python](https://github.com/carmiac/tca-python)
- [TCA-Go](https://github.com/carmiac/tca-go)

### Themes

TCA uses base24 YAML themes maintained by [Tinted Theming](https://github.com/tinted-theming/home). Any base24 theme works with TCA automatically.

### Shared Theme Directory

TCA uses an XDG compliant shared directory for themes:

- Linux/BSD: `$XDG_DATA_HOME/tca/themes` (default: `~/.local/share/tca/themes`)
- macOS: `~/Library/Application Support/tca/themes`
- Windows: `%APPDATA%\tca\themes`

Themes in this directory can be loaded by name from any TCA-powered application.

### Managing Themes

```bash
# Add a theme from Tinted Theming by name
tca add "Tokyo Night"

# List all available themes
tca list

# Validate a theme file
tca validate nord-dark.yaml
```

### Setting Your Default Theme

```bash
# Set a default for all TCA-powered apps
tca config set default "Tokyo Night"

# Set separate defaults for dark and light mode
tca config set dark "Tokyo Night"
tca config set light "Solarized Light"
```

TCA-powered apps automatically pick up your preference — no per-app configuration needed.

### Export to Your Terminal

TCA uses [Tinted Theming](https://github.com/tinted-theming/home) for export. Install the `tinty` tool and use it to export any base24 theme to your terminal, editor, or UI framework.

### Preview with the Picker

```bash
cd ../tca-ratatui
cargo run --example picker
```

## For Application Developers

Integrating TCA into your app takes one line. Your users get access to every theme they have installed — and 11 built-in themes — with no extra work from you.

### Rust / Ratatui

```toml
# Cargo.toml
[dependencies]
tca-ratatui = "0.3"
```

```rust
use tca_ratatui::TcaTheme;

// Loads the user's preferred theme, falls back to a built-in automatically
let theme = TcaTheme::new(None);

// Use semantic color names — not hardcoded values
let style     = Style::default().fg(theme.ui.fg_primary).bg(theme.ui.bg_primary);
let error     = Style::default().fg(theme.semantic.error);
let border    = Style::default().fg(theme.ui.border_primary);
let selection = Style::default().fg(theme.ui.selection_fg).bg(theme.ui.selection_bg);
```

Your app never needs to know which theme is active. Semantic names (`error`, `fg_primary`, `border_primary`) stay stable across every theme your users install.

### Go / Lipgloss

```go
import "github.com/carmiac/tca-go"

theme := tca.NewTheme(nil) // nil = use user preference

style := lipgloss.NewStyle().
    Foreground(theme.UI.FgPrimary).
    Background(theme.UI.BgPrimary)
```

### Python / Textual

```python
from tca import TcaTheme

theme = TcaTheme()  # loads user preference automatically

error_color = theme.semantic.error
```

### Theme Resolution

All TCA libraries resolve themes in the same order:

1. The name or path passed explicitly by the app
2. The user's preference file (`~/.config/tca/tca.toml`)
3. Dark/light mode auto-detection
4. A built-in default

Pass `None`/`nil` to skip step 1 and let the user's preference take over entirely.

## Contributing

Contributions to the specification and tca-cli are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).
