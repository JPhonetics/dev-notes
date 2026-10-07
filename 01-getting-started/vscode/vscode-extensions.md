---
Created: 2026-10-06
Modified:
---

# Recommended VS Code Extensions

VS Code extensions add optional features and integrations to the editor, including language support, linting, formatting, debugging, framework tooling, source control enhancements, database tools, and other development utilities.

Extensions are not required to use VS Code, but they can make development more efficient by adding features tailored to the languages, frameworks, and tools used in a project. Because different projects require different workflows, extensions should be installed based on actual development needs rather than treated as mandatory.

This guide documents the recommended VS Code extensions used throughout this development environment and explains what each extension provides so they can be installed selectively as needed.

## Overview

1. [Recommended Extensions](#recommended-extensions)

## Recommended Extensions

> [!IMPORTANT] 
> Click the **Extensions** icon in the left panel to search and install extensions.

### <u>API Development</u>

- **REST Client** (`humao.rest-client`)
  - Allows HTTP requests to be written and executed directly inside VS Code using `.http` or `.rest` files. It supports common request methods such as `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`, along with headers, authentication, request bodies, and environment variables, making it useful for quickly testing and debugging APIs without opening a separate application such as Postman.

### <u>Docker / Containers</u>

- **Container Tools** (`ms-azuretools.vscode-containers`)
   - Adds support for building, running, managing, and debugging containerized applications directly from VS Code. It supports Docker-based workflows including containers, images, Dockerfiles, Compose files, logs, and related resources, and replaced Microsoft’s older Docker extension, making it the current Microsoft extension for general container development.

### <u>Git</u>

- **GitLens** (`eamodio.gitlens`)
   - Enhances VS Code’s built-in Git functionality with detailed commit history, file history, blame information, branch insights, repository exploration, and other source-control tools. It is useful for understanding how a project has changed over time and quickly identifying who changed a line of code, when it changed, and which commit introduced it.

### <u>JavaScript</u>

- **ESLint** (`dbaeumer.vscode-eslint`)
   - Integrates ESLint directly into VS Code so JavaScript and TypeScript linting problems can be identified while writing code. It is useful for catching potential errors, enforcing project coding standards, and displaying project-specific linting rules directly in the editor before code is executed or committed.

- **Prettier - Code formatter** (`esbenp.prettier-vscode`)
   - Automatically formats supported files according to consistent formatting rules for spacing, indentation, line breaks, quotes, and other style decisions. It is especially useful for JavaScript, React, JSON, CSS, and other web-development files because it keeps formatting consistent across a project without requiring developers to manually style every file.

### <u>Python</u>

- **Python** (`ms-python.python`)
  - Adds core Python development support to VS Code, including interpreter selection, virtual environment integration, testing support, debugging integration, and other Python-specific development features. It also works with supporting Microsoft extensions such as **Pylance** and **Python Debugger**, making it the primary extension needed for Python development in VS Code.

- **Django** (`batisteo.vscode-django`)
   - Adds Django-specific support such as template syntax highlighting, snippets, and language features for Django projects. It is useful when actively developing with Django because it improves support for Django templates and common framework patterns that the general Python extension does not specifically target.

### <u>SQL</u>

- **SQLTools** (`mtxr.sqltools`)
   - Adds database connections, schema browsing, SQL query execution, query history, and other database-development features directly inside VS Code. It is useful for quickly inspecting databases and executing SQL without leaving the editor.

### <u>Utilities</u>

- **Code Spell Checker** (`streetsidesoftware.code-spell-checker`)
   - Checks spelling in source code, comments, documentation, Markdown, variable names, and other text inside a project. It is useful for catching typos in documentation, identifiers, API names, comments, and other places where normal spell-checking tools usually do not operate.

- **Live Server** (`ritwickdey.liveserver`)
  - Starts a lightweight local web server and automatically refreshes the browser when HTML, CSS, or JavaScript files change. It is especially useful for quickly previewing and testing static web projects without manually refreshing the browser after each change.

- **Live Share** (`ms-vsliveshare.vsliveshare`)
  - Allows other developers to join a VS Code development session remotely and collaborate on the same project in real time. It is useful for pair programming, tutoring, collaborative debugging, and reviewing code together without requiring every participant to independently set up the entire development environment.
   
## Related Documentation

- [Install Visual Studio Code](./install-vscode.md)
- [Windows Development Setup](../setup-windows.md)