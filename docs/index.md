---
layout: home

hero:
  eyebrow: "Terminal Colors Architecture"
  title: "Your theme."
  title_accent: "Every terminal. Every CLI. Every TUI."
  tagline: >
    TCA is a structured, portable format for terminal and CLI/TUI color themes. Define your palette once in TOML then export to any terminal, editor, or UI framework.


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
        One line of code, zero configuration. Your app immediately supports built-in themes and any theme your users install with no config parsers to write, no theme files to ship. Sane fallbacks and defaults are included in the reference libraries.


    - icon: "🎨"
      title: "Theme Authors"
      body: >
        Write one .toml file and reach every TCA-powered app across Rust, Python, and Go. Built-in validation catches contrast issues and broken color references before you ship.


    - icon: "🛼"
      title: "Theme Users"
      body: >
        Drop any .toml file into ~/.local/share/tca/themes/ and every TCA-powered app on your system can use it with no per-app configuration needed. Set your preferred theme once, not every time.


ecosystem:
  label: "Ecosystem"
  heading: "Libraries &amp; Tools"
  intro: "TCA themes work across languages and frameworks. Pick the library for your stack."
  cards:
    - icon: "🎨"
      title: "tca-themes"
      url: "https://github.com/carmiac/tca-themes"
      body: "Ready-to-use theme collection and the canonical JSON schema for validation."
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
  intro: "Install the CLI, validate a theme, and export to your terminal in under a minute."
  blocks:
    - title: "CLI"
      content: |
        <span class="comment"># Install</span>
        <span class="prompt">$</span> cargo install tca-cli

        <span class="comment"># Validate a theme</span>
        <span class="prompt">$</span> tca validate dracula

        <span class="comment"># List installed themes</span>
        <span class="prompt">$</span> tca list

        <span class="comment"># Export to your terminal</span>
        <span class="prompt">$</span> tca export nord-dark -f kitty -o ~/.config/kitty/theme.conf
    - title: "Rust"
      content: |
        <span class="comment"># Cargo.toml</span>
        tca-ratatui = "0.3"

        <span class="comment">// main.rs</span>
        let theme = TcaTheme::new(None);
        let style = Style::default()
            .fg(theme.ui.fg_primary)
            .bg(theme.ui.bg_primary);

current_state:
  label: "Current State"
  heading: "Draft — Public Input Welcome"
  body: >
    TCA is a draft specification under active development. Core features are working: language libraries, theme validation, and import/export. We are seeking feedback before finalising the v1.0 specification.


  roadmap:
    - "Expanded tca tools for import, export, and automatic theme downloads"
    - "Per-user theme preference library support"
    - "More themes"

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
