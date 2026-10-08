# Dev Notes

A personal software development knowledge base containing setup guides, technical concepts, language and framework notes, reusable references, troubleshooting documentation, and project-specific learnings.

The goal of this repository is to keep development knowledge organized, easy to search, understandable, and reusable over time. Documentation should serve as both a practical reference and a learning resource, explaining not only how to perform a task but also what the tools and commands do and why they are used.

## Getting Started

The recommended starting point is the Windows development environment setup, which combines Windows applications with Linux development tools through Windows Subsystem for Linux (WSL).

The setup covers Ubuntu, Python, JavaScript, Git, GitHub CLI, Docker, Visual Studio Code, database and API clients, and optional AI-assisted development tools.

📌 [Setup Windows for Development](./01-getting-started/setup-windows.md)

## Structure

### 01. Getting Started

Environment and setup guides used to prepare a development system.

Examples:

- Windows development setup
- WSL and Ubuntu
- Windows Terminal
- Python and JavaScript
- Git and GitHub CLI
- Docker
- Visual Studio Code
- DBeaver and Postman
- Claude Code and Codex CLI

[Browse Getting Started](./01-getting-started/)

### 02. Concepts

Explanations of technical concepts, how they work, and why they are used.

Examples:

- APIs
- HTTP
- Cookies
- Sessions
- Authentication
- Webhooks
- Databases
- Networking
- Artificial intelligence

[Browse Concepts](./02-concepts/)

### 03. Languages and Runtimes

Notes about programming languages, runtimes, syntax, behavior, standard libraries, and language-specific patterns.

Examples:

- Python
- JavaScript
- SQL
- Bash
- PowerShell
- Node.js

[Browse Languages and Runtimes](./03-languages-and-runtimes/)

### 04. Frameworks and Libraries

Notes about frameworks, libraries, SDKs, and other development technologies.

Examples:

- Django
- Django REST Framework
- React
- Flask
- Stripe
- Testing libraries

[Browse Frameworks and Libraries](./04-frameworks-and-libraries/)

### 05. Guides

Step-by-step guides for accomplishing specific development tasks beyond the initial environment setup.

Examples:

- Build a REST API
- Configure Django with PostgreSQL
- Integrate Stripe with Django
- Add authentication
- Dockerize an application
- Deploy a full-stack application

[Browse Guides](./05-guides/)

### 06. Reference

Quick-reference material and reusable resources for commonly used commands, configurations, and development workflows.

Examples:

- Code snippets
- Scripts
- SQL queries
- Commands
- Templates
- Cheat sheets

[Browse Reference](./06-reference/)

### 07. Troubleshooting

Documentation for errors, problems, fixes, and lessons learned while developing or configuring tools.

Examples:

- WSL issues
- Docker problems
- Git errors
- Django errors
- PostgreSQL issues
- Environment configuration problems

[Browse Troubleshooting](./07-troubleshooting/)

### 08. Project Notes

Notes that are specific to individual projects, experiments, or implementations.

General knowledge discovered during a project should eventually be moved or rewritten into the appropriate section of this repository.

[Browse Project Notes](./08-project-notes/)

## Documentation Principles

### Keep Documentation DRY

Avoid unnecessarily duplicating complete instructions or explanations across multiple files.

Create one canonical guide or explanation for a topic and reference it from related documentation.

For example:

```text
setup-windows.md
    ↓
wsl/install-wsl.md
    ↓
wsl/commands.md
```

The Windows setup guide defines the overall installation order, the dedicated WSL guide contains the detailed installation instructions, and the WSL commands reference provides reusable command information.

Documentation should still be understandable when read independently. Common commands, options, and concepts can be briefly reintroduced when needed, without requiring readers to navigate to another document for basic explanations.

Within an individual guide, introduce recurring concepts once and focus subsequent explanations on new commands, options, packages, and behaviors.

### Keep Notes Focused

Each file should have a clear purpose.

Use:

- **Getting Started** for environment setup
- **Concepts** for understanding how something works
- **Languages and Runtimes** for language-specific knowledge
- **Frameworks and Libraries** for technology-specific knowledge
- **Guides** for completing a development task
- **Reference** for fast lookup and reusable material
- **Troubleshooting** for problems and fixes
- **Project Notes** for project-specific information

### Prefer Shallow Folder Structures

Avoid unnecessary nesting.

Use the shallowest folder structure that still makes it obvious where documentation belongs.

For example:

```text
01-getting-started/
└── wsl/
    ├── install-wsl.md
    └── commands.md
```

Instead of:

```text
01-getting-started/
└── operating-systems/
    └── windows/
        └── linux/
            └── wsl/
                └── install-wsl.md
```

### Separate Concepts From Procedures

A conceptual document explains **what something is, why it matters, and how it works**.

A guide explains **how to do something**, including the commands, configurations, and verification steps required to complete a task.

For example:

```text
02-concepts/
└── web/
    └── cookies.md
```

Explains how cookies work.

While:

```text
05-guides/
└── authentication/
    └── django-session-authentication.md
```

Explains how to implement session authentication.

Guides may introduce necessary concepts without duplicating an entire conceptual document.

### Use Consistent File Names

Use lowercase kebab-case for folders and filenames.

Examples:

```text
install-wsl.md
virtual-environments.md
django-authentication.md
docker-networking.md
```

### Use Consistent Markdown Formatting

Documentation should use consistent formatting wherever practical.

Common conventions include:

- `#` for the document title
- `##` for major sections
- `###` for subsections when needed
- **Bold** for application names, environments, UI labels, and important concepts
- `Inline code` for commands, options, filenames, paths, literal values, and output fields
- Code blocks for executable commands, configurations, source code, and terminal output
- Numbered steps for procedures
- Bullets for explanations, references, and supporting information
- GitHub callouts for supplemental information, tips, important requirements, and warnings

Example:

```markdown
> [!NOTE]
> Additional context or reference information.

> [!IMPORTANT]
> Information required to successfully complete a task.

> [!TIP]
> A useful shortcut or recommendation.

> [!WARNING]
> Information that may prevent an error or unwanted behavior.
```

### Explain Commands and Configuration

Installation guides should explain the commands and configurations being used rather than simply providing instructions to copy and paste.

When appropriate, explain:

- What a command or tool does
- What its options and arguments mean
- What packages or dependencies are being installed
- Why a configuration is needed
- What behavior or result to expect

Keep explanations directly beneath the relevant procedural step using indented bullets.

Introduce recurring concepts once per guide and avoid repeating explanations unnecessarily within the same document.

Prioritize understanding over exhaustive syntax explanations. More advanced language features, shell behavior, and configuration details can be documented separately when needed.

### Keep Installation Guides Consistent

Installation guides should follow a predictable structure whenever applicable:

1. **Introduction** — Explain what the tool is, why it is useful, and what the guide installs or configures.
2. **Overview** — Provide links to the major sections of the guide.
3. **Installation and Configuration** — Present numbered actions, command explanations, and executable commands.
4. **Verification** — Confirm that the tool was installed or configured successfully.
5. **Related Documentation** — Link to related guides within the repository.
6. **Official Documentation** — Provide authoritative external references.

Use GitHub callouts when additional context is helpful, but keep essential command explanations alongside their corresponding steps.

Not every guide requires every section. Use the structure where it improves clarity rather than adding sections solely for consistency.

## Metadata

Documentation files may include YAML front matter for basic metadata.

Example:

```yaml
---
Created: 2026-10-05
Modified:
Tags: [development, install, windows, wsl]
```

Tags should remain concise and reusable across the repository.

## Repository Philosophy

These notes are intended to function as a personal developer handbook and long-term learning reference rather than temporary class notes.

Documentation should be useful when revisited months or years later and should explain enough context to understand:

- What something is
- Why it is used
- How it fits into the larger development environment
- What commands, options, and configurations accomplish
- How to install, configure, verify, or implement it
- How to troubleshoot common problems
- Where to find related documentation

The goal is to preserve both practical instructions and the reasoning behind them, making it easier to understand, maintain, and build upon previous work.