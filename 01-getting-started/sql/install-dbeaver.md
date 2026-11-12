---
Created: 2026-10-06
Modified: 2026-11-10
---

# Install DBeaver

DBeaver is a graphical database management application used to connect to, explore, query, and manage relational databases from a single interface. It supports multiple database systems, including PostgreSQL, MySQL, MariaDB, SQLite, SQL Server, and others through database-specific drivers.

DBeaver is useful for development because it provides a consistent graphical interface for viewing schemas, tables, relationships, data, and query results without requiring a separate management application for each database system. It can connect to databases running locally, remotely, or inside Docker containers.

This guide installs DBeaver on Windows and prepares it for connecting to databases used throughout development.

## Overview

1. [Install DBeaver](#install-dbeaver)
2. [Verify Installation](#verify-installation)
3. [Related Documentation](#related-documentation)
4. [Official Documentation](#official-documentation)

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

3. Complete the **Product Configuration** prompts. These settings can be changed later under **Help > Show Product Configuration**.

> [!NOTE]
> When connecting to a database engine for the first time, DBeaver may prompt you to download the required database-specific JDBC driver. The driver allows DBeaver to communicate with that database system.

[🔼 Back to Overview](#overview)

## Verify Installation

1. Open **DBeaver**.

2. Create a sample SQLite database using **Help > Create Sample Base**, if one was not created during the initial setup.

3. Locate the sample database in the **Database Navigator** and connect to it.

4. Open a new SQL editor for the sample database.

5. Execute a SQL query to retrieve records from the sample database. The query should execute successfully and display up to 10 customer records in the results.

    - `SELECT * FROM Customer LIMIT 10;` retrieves up to 10 records from the `Customer` table.
        - `SELECT` retrieves data from a database.
        - `*` selects all columns.
        - `FROM Customer` identifies the table to query.
        - `LIMIT 10` restricts the results to a maximum of 10 rows.

    ```sql
    SELECT *
    FROM Customer
    LIMIT 10;
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [DBeaver Documentation](https://dbeaver.com/docs/dbeaver/) — Official documentation for installing, configuring, and using DBeaver to manage databases, execute SQL queries, and work with database connections.

---

[🔼 Back to Getting Started](../README.md)