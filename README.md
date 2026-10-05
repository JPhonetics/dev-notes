# Dev Notes

A personal software development knowledge base containing setup guides, technical concepts, language and framework notes, reusable references, troubleshooting documentation, and project-specific learnings.

The goal of this repository is to keep development knowledge organized, easy to search, and reusable over time.

## Structure

### 01. Getting Started

Environment and setup guides used to prepare a development system.

Examples:

- Windows development setup
- WSL
- Git
- Python
- Node.js
- Visual Studio Code
- Docker
- PostgreSQL

[Browse Getting Started](./01-getting-started/)

### 02. Concepts

Explanations of technical concepts and how they work.

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

Step-by-step guides for accomplishing specific development tasks.

Examples:

- Build a REST API
- Configure Django with PostgreSQL
- Integrate Stripe with Django
- Add authentication
- Dockerize an application
- Deploy a full-stack application

[Browse Guides](./05-guides/)

### 06. Reference

Quick-reference material and reusable resources.

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

Avoid duplicating the same instructions or explanations across multiple files.

Create one canonical guide or explanation and reference it from other documentation.

For example:

```text
setup-windows.md
    ↓
wsl/install-wsl.md
    ↓
reference to commands.md
```

The Windows setup guide defines the overall setup order, while the dedicated WSL guide contains the detailed installation instructions.

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

instead of:

```text
01-getting-started/
└── operating-systems/
    └── windows/
        └── linux/
            └── wsl/
                └── install-wsl.md
```

### Separate Concepts From Procedures

A conceptual document explains **what something is and how it works**.

A guide explains **how to do something**.

For example:

```text
02-concepts/
└── web/
    └── cookies.md
```

explains how cookies work.

While:

```text
05-guides/
└── authentication/
    └── django-session-authentication.md
```

explains how to implement session authentication.

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
- **Bold** for application names, environments, UI labels, and important concepts
- `Inline code` for commands, flags, filenames, paths, literal values, and output fields
- Code blocks for commands, configuration, code, and terminal output
- GitHub callouts for notes, tips, important information, and warnings

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

## Metadata

Documentation files may include YAML front matter for basic metadata.

Example:

```yaml
---
Created: 2026-10-05
Modified:
Tags: [development, install, windows, wsl]
---
```

Tags should remain concise and reusable across the repository.

## Repository Philosophy

These notes are intended to function as a personal developer handbook rather than temporary class notes.

Documentation should be useful when revisited months or years later and should explain enough context to understand:

- What something is
- Why it is used
- How it fits into the larger development environment
- How to configure or implement it
- Where to find related documentation