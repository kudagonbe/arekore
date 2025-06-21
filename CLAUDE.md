# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a personal configuration documentation repository (`arekore`) that contains detailed documentation of various development environment settings in Japanese. The repository serves as a centralized knowledge base for personal development tool configurations.

## Repository Structure

```
arekore/
├── settings/           # Configuration documentation
│   ├── nvim.md        # Neovim configuration summary
│   ├── wezterm.md     # WezTerm terminal configuration summary  
│   └── zsh.md         # Zsh shell configuration summary
└── [generated files]   # Files created during documentation sessions
```

## Key Characteristics

### Language and Localization
- All documentation is written in **Japanese**
- Configuration files reference Japanese input methods (IME)
- Comments and descriptions use Japanese terminology

### Content Focus
- **Development Environment Documentation**: Comprehensive summaries of personal tool configurations
- **Setup Instructions**: Step-by-step installation and configuration guides
- **Dependency Management**: Clear documentation of required tools and packages

### Development Environment Stack
The documented configurations cover:
- **Terminal**: WezTerm with custom themes and transparency
- **Shell**: Zsh with advanced prompt, history management, and productivity tools
- **Editor**: Neovim with LSP support, airline themes, and vim-plug
- **Version Management**: asdf for language version management
- **Productivity Tools**: lazygit, pet (command snippets), extensive aliases

## Working with This Repository

### File Creation Patterns
- New configuration documentation should follow the existing naming pattern: `[tool].md`
- Place configuration summaries in the `settings/` directory
- Maintain Japanese language consistency across all documentation

### Documentation Style
- Include configuration file locations (absolute paths)
- Document required dependencies and installation commands
- Provide setup verification commands
- Explain key features and customizations
- Include relevant color codes, themes, and visual settings

### Common Operations
- When adding new tool documentation, analyze the actual configuration files first
- Always include installation prerequisites and dependency information
- Maintain the established Japanese documentation format
- Include command examples with proper syntax highlighting