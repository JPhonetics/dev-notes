---
Created: 2026-10-06
Modified:
---

# Install DBeaver

DBeaver is a graphical database management application used to connect to, explore, query, and manage relational databases from a single interface. It supports multiple database systems, including PostgreSQL, MySQL, MariaDB, SQLite, SQL Server, and others through database-specific drivers.

DBeaver is useful for development because it provides a consistent graphical interface for viewing schemas, tables, relationships, data, and query results without requiring a separate management application for each database system. It can connect to databases running locally, remotely, or inside Docker containers.

This guide installs DBeaver on Windows and prepares it for connecting to databases used throughout development.

## Overview

1. [Install DBeaver](#install-dbeaver)

## Install DBeaver

> [!TIP]
> DBeaver can create a sample SQLite database during the initial setup. This is optional, but useful for exploring the interface, viewing tables, and practicing SQL queries before connecting to a development database.

1. Download and install [DBeaver](https://dbeaver.io/download/).
   1. **Choose Users:** 
      - For me
   2. **Choose Components:** 
      - ✅ Include Java
      - ✅ Associate SQL files
      - ✅ Associate SQLite database files
      - ❌ Add Microsoft Defender Antivirus exclusion
      - ❌ Reset settings
2. Launch **DBeaver**.
3. Complete the **Product Configuration** prompts. These settings can be changed later under **Help > Product Configuration**.

> [!NOTE]
> When connecting to a database engine for the first time, DBeaver may prompt you to download the required database-specific JDBC driver. The driver allows DBeaver to communicate with that database system.

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [DBeaver Documentation](https://dbeaver.com/docs/dbeaver/)