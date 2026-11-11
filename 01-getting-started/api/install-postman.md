---
Created: 2026-10-07
Modified: 2026-11-10
---

# Install Postman

Postman is an API development and testing application used to send HTTP requests, inspect responses, manage authentication, organize request collections, and work with APIs from a graphical interface.

Postman is useful for development because it allows requests, headers, parameters, request bodies, authentication settings, and environments to be saved and reused instead of being recreated manually. It is especially helpful when building or debugging backend services, REST APIs, and integrations with external services.

This guide installs Postman, explains the optional account sign-in, and verifies that the application is ready for API testing.

## Overview

1. [Install Postman](#install-postman)
2. [Verify Installation](#verify-installation)
3. [Related Documentation](#related-documentation)
4. [Official Documentation](#official-documentation)

## Install Postman

1. Download **Postman** from the [official website](https://www.postman.com/downloads/).
2. Run the downloaded installer and follow the installation prompts.
3. Launch **Postman**. The application may open automatically after installation.
4. Sign in to a **Postman** account or continue without signing in.

    - **Without an account:** Postman provides a lightweight API Client for creating and sending requests, viewing responses, and accessing local request history. Work is stored locally and is not synchronized with Postman Cloud.
    - **With an account:** Postman enables additional features, including:
        - Saving and organizing requests into Collections.
        - Managing environments and variables.
        - Synchronizing work across devices.
        - Sharing Collections and collaborating through Workspaces.

[🔼 Back to Overview](#overview)

## Verify Installation

1. Open **Postman**.

2. Create a new HTTP request.

3. Select **GET** as the request method.

    - `GET` is an HTTP method used to retrieve information from a server without requesting changes to its data.

4. Enter the following URL into the request URL field.

    - `https://postman-echo.com/get` is a test endpoint provided by Postman that returns information about the request it receives.

    ```text
    https://postman-echo.com/get
    ```

5. Click **Send**.

    Postman should display a successful `200 OK` response and a JSON response body containing information about the request.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Postman Documentation](https://learning.postman.com/docs/) — Official documentation for configuring and using Postman for API development and testing.
- [Postman Installation](https://learning.postman.com/docs/getting-started/installation/install-app) — Official instructions for installing Postman on Windows, macOS, and Linux.
- [Postman API Client Documentation](https://learning.postman.com/docs/sending-requests/requests/) — Official guide for creating and sending HTTP requests, configuring headers, parameters, request bodies, and authentication, and inspecting API responses.

---

[🔼 Back to Getting Started](../README.md)