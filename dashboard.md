# Project: Foundational WHMCS Replacement in Rust

### High-Level Objective

The objective of this project is to build the foundational first version of a modern, self-sufficient server lifecycle management system in Rust, designed as a strategic replacement for the legacy WHMCS platform. The core of the project will involve creating a foundational API for server management and demonstrating a critical feature: a one-time data migration from a live WHMCS instance.

### Core Project Deliverables

This project will produce three key, interconnected components that establish the new system's viability:

*   **A Configured WHMCS Testbed:** A local environment simulating the legacy system, serving as the data source for the migration.
*   **A Core Server Management API (Rust):** The foundational microservice that will handle all future server lifecycle operations.
*   **A Standalone Migration Utility (Rust):** A command-line tool to prove the viability of migrating critical data from WHMCS to the new system.

### WHMCS Testbed Setup

A local WHMCS instance must be set up and populated with complex sample data to accurately simulate the production environment. This includes:

*   **Users and Services:** A mix of users and server products with various statuses (`Active`, `Inactive`, `Suspended`).
*   **Complex Server Specifications:** Crucially, several active servers must be configured with **Configurable Options** (e.g., RAM, CPU Cores) and **Custom Fields** (e.g., `OS_Image`, `DataCenter_Location`), as this data represents the primary migration challenge.

### Core API Service (Rust)

This is the foundational REST API that will form the heart of the new system. The focus is on implementing the essential features correctly.

*   **API Architecture:** Built with `axum`, using `PostgreSQL` for persistence (via `sqlx` or `diesel`), `sqlx-cli` for database migrations, and `tracing` for structured logging.
*   **Essential Functionality:**
    *   **JWT Authentication:** A public `/login` endpoint will issue tokens, and all other endpoints must be protected.
    *   **Asynchronous Provisioning:** Server creation (`POST /servers`) must be handled asynchronously, immediately returning a `202 Accepted` response while a background worker handles the (mocked) provisioning logic.
    *   **Key Lifecycle Endpoints:** A minimal set of endpoints to manage the server lifecycle (`/login`, `/users/me`, `POST /servers`, `GET /servers/:id`).
*   **External Service Mocking:** A `ProxmoxClient` trait will be defined and mocked to simulate interactions with an external hypervisor, including realistic delays and potential failures.

### Migration Utility (Rust)

A separate, standalone Rust binary will be created to demonstrate a safe and effective data migration process.

*   **Direct Database Extraction:** The utility will connect directly to the WHMCS MariaDB/MySQL database to extract user and server data.
*   **Core Transformation Logic:** The script's primary purpose is to prove it can migrate active users and their servers. The most critical task is to correctly extract and transform data from WHMCS's **Configurable Options and Custom Fields** into the new, structured PostgreSQL schema.
*   **Essential Safeguards:** To prove the migration strategy is viable and safe, the utility must include:
    *   **Idempotency:** The script must be safe to run multiple times without creating duplicate data.
    *   **A `--dry-run` flag:** This will print a summary of actions that would be taken, allowing for validation without committing any changes.

### Quality and Validation

To validate the success of this initial build, a foundational test suite is required, focusing on the most critical components:

*   **Unit Tests:** Primarily for the migration's transformation logic, ensuring the mapping from WHMCS fields to the new schema is correct.
*   **Integration Tests:** To verify the core API "happy path," including successful authentication and the correct asynchronous handling of a server creation request.
