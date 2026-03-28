---
layout: home

hero:
  eyebrow: "Terminal Colors Architecture"
  title: "Your theme."
  title_accent: "Every terminal. Every CLI. Every TUI."
  tagline: >
    TCA is a structured, portable format for terminal and CLI/TUI color themes. Define your theme once as a base24 YAML file then use it everywhere.


  install: "cargo install tca-cli"
  links:
    - text: "GitHub"
      url: "https://github.com/carmiac/tca-themes"
    - text: "Specification"
      url: "https://github.com/carmiac/tca-themes/blob/main/SPECIFICATION.md"
    - text: "Contribute a Theme"
      url: "https://github.com/carmiac/tca-themes/blob/main/CONTRIBUTING.md"

audience:
  label: "Who is This For?"
  heading: "Built for Developers, Theme Authors, and Users"
  intro: "TCA makes color themes easier for everyone involved."
  cards:
    - icon: "💻"
      title: "Application Developers"
      body: >
        One line of code, zero configuration. Your app immediately supports built-in themes and any theme your users install. No config parsers to write, no theme files to ship, no hardcoded colors. Use semantic names like error, fg_primary, and border_primary. They stay stable across every theme.


    - icon: "🎨"
      title: "Theme Authors"
      body: >
        Write one base24 YAML file and reach every TCA-powered app across Rust, Python, and Go. Built-in validation catches contrast issues and file issues before you ship.


    - icon: "🛼"
      title: "Theme Users"
      body: >
        Drop any base24 YAML file into ~/.local/share/tca/themes/ and every TCA-powered app on your system can use it with no per-app configuration needed. Or download them all with a single command. Set your preferred theme once, not every time.


ecosystem:
  label: "Ecosystem"
  heading: "Libraries &amp; Tools"
  intro: "TCA themes work across languages and frameworks. Pick the library for your stack."
  cards:
    - icon: "🎨"
      title: "tca-themes"
      url: "https://github.com/carmiac/tca-themes"
      body: "Canonical theme specification."
    - icon: "🦀"
      title: "tca-rust"
      url: "https://github.com/carmiac/tca-rust"
      body: "Core Rust crates: CLI tool, type definitions, theme loader, and Ratatui widgets."
    - icon: "🐹"
      title: "tca-go"
      url: "https://github.com/carmiac/tca-go"
      body: "Go library with Lipgloss integration for colorful terminal applications."
    - icon: "🐍"
      title: "tca-python"
      url: "https://github.com/carmiac/tca-python"
      body: "Python library with Textual integration for colorful terminal applications."

quickstart:
  label: "Quick Start"
  heading: "Get Going Fast"
  intro: "Add a theme, set your default, and integrate into your app in minutes."
  blocks:
    - title: "CLI"
      content: |
        <span class="comment"># Install</span>
        <span class="prompt">$</span> cargo install tca-cli

        <span class="comment"># Add a theme and set it as default</span>
        <span class="prompt">$</span> tca add "Tokyo Night"
        <span class="prompt">$</span> tca config set default "Tokyo Night"

        <span class="comment"># Add all the themes</span>
        <span class="prompt">$</span> tca add --all

        <span class="comment"># List available themes</span>
        <span class="prompt">$</span> tca list

        <span class="comment"># Validate a theme file</span>
        <span class="prompt">$</span> tca validate nord-dark.yaml
    - title: "Rust"
      content: |
        <span class="comment">// Cargo.toml: tca-ratatui = "0.3"</span>

        <span class="comment">// Loads user preference, falls back to built-in</span>
        let theme = TcaTheme::default();

        <span class="comment">// Load a theme by name, falls back to built-in</span>
        let theme = TcaTheme::new(Some("tokyo night");

        <span class="comment">// Get a cursor to move through all installed themes.
        let cursor = TcaThemeCursor::with_all_themes();

        <span class="comment">// Semantic names — stable across all themes</span>
        let style = Style::default()
            .fg(theme.ui.fg_primary)
            .bg(theme.ui.bg_primary);
        let error  = Style::default().fg(theme.semantic.error);
        let border = Style::default().fg(theme.ui.border_primary);
    - title: "Go / Python"
      content: |
        <span class="comment">// Go + Lipgloss</span>
        theme := tca.NewTheme(nil)
        style := lipgloss.NewStyle().
            Foreground(theme.UI.FgPrimary).
            Background(theme.UI.BgPrimary)

        <span class="comment"># Python + Textual</span>
        theme = TcaTheme()
        error_color = theme.semantic.error

current_state:
  label: "Current State"
  heading: "Draft — Public Input Welcome"
  body: >
    TCA is a draft specification under active development. Core features are working: language libraries, theme validation, and import/export. I am seeking feedback before finalising the v1.0 specification.


footer:
  links:
    - text: "tca-themes"
      url: "https://github.com/carmiac/tca-themes"
    - text: "tca-rust"
      url: "https://github.com/carmiac/tca-rust"
    - text: "tca-go"
      url: "https://github.com/carmiac/tca-go"
    - text: "tca-python"
      url: "https://github.com/carmiac/tca-python"
    - text: "Specification"
      url: "https://github.com/carmiac/tca-themes/blob/main/SPECIFICATION.md"
  license: "Public Domain (Unlicense)"
---
