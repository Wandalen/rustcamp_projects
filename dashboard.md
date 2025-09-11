# Project: Full-Stack Proxmox Management Platform in Rust & React

### High-Level Objective

The objective of this project is to build a complete, user-facing, and self-sufficient server lifecycle management system. This involves creating a foundational API in Rust that directly orchestrates a Proxmox VE instance, building an interactive React dashboard for user interaction, and demonstrating a critical data migration from a legacy WHMCS platform. The result will be a vertically integrated, full-stack replacement for the core functionality of WHMCS.

### Core Project Deliverables

This project will produce five key, interconnected components that establish the new platform's viability:

*   **A Configured WHMCS Testbed:** A local environment simulating the legacy system, serving as the data source for the migration.
*   **A Proxmox VE Testbed:** A local virtualization environment that the new system will actively manage.
*   **A Core Server Management API (Rust):** The orchestration microservice that handles all server lifecycle operations by communicating directly with Proxmox.
*   **An Interactive Frontend Dashboard (React):** A modern web interface for users to log in, view, and manage their servers.
*   **A Standalone Migration Utility (Rust):** A command-line tool to prove the viability of migrating critical data from WHMCS to the new system.

### Environment Setup

#### 1. WHMCS Testbed Setup
A local WHMCS instance must be set up and populated with complex sample data to accurately simulate the production environment. This includes:
*   **Users and Services:** A mix of users and server products with various statuses (`Active`, `Inactive`, `Suspended`).
*   **Complex Server Specifications:** Crucially, several active servers must be configured with **Configurable Options** (e.g., RAM, CPU Cores) and **Custom Fields** (e.g., `OS_Image_Template`, `DataCenter_Location`), as this data represents the primary migration challenge and will inform real provisioning parameters.

#### 2. Proxmox VE Testbed Setup
A local Proxmox VE instance must be configured to act as the target hypervisor. This setup must include:
*   **A running Proxmox VE node** accessible from the Rust API service.
*   **At least one cloneable VM template** (e.g., a Cloud-Init ready Ubuntu image) that the API will use to provision new servers.

### Core API Service & Proxmox Orchestrator (Rust)

This is the REST API that will form the heart of the new system, responsible for all business logic and infrastructure orchestration.

*   **API Architecture:** Built with `axum`, using `PostgreSQL` for persistence (via `sqlx` or `diesel`), `sqlx-cli` for database migrations, and `tracing` for structured logging.
*   **Essential Functionality:**
    *   **JWT Authentication:** A public `/login` endpoint will issue tokens, and all other endpoints must be protected.
    *   **Asynchronous Provisioning:** Server creation (`POST /servers`) must be handled asynchronously, immediately returning a `22 Accepted` response while a background worker orchestrates the **real provisioning process on Proxmox**.
    *   **Full Lifecycle Endpoints:** A comprehensive set of endpoints to manage the server lifecycle (`/login`, `/users/me`, `GET /servers`, `POST /servers`, `GET /servers/:id`, `POST /servers/:id/actions`, `DELETE /servers/:id`).
*   **Real Proxmox Integration:** The service must directly integrate with the Proxmox VE API to perform all lifecycle actions. This includes cloning VMs from templates, starting/stopping/rebooting VMs, and deleting them upon termination.

### Interactive Frontend Dashboard (React)

A modern Single-Page Application (SPA) will serve as the primary user interface for the platform.

*   **Technology Stack:** Built with **React (using Vite)**. Use of a modern UI component library is encouraged.
*   **Core Features:**
    *   **Secure Authentication:** A login page to authenticate against the API, securely manage the JWT, and protect user-facing routes.
    *   **Server Dashboard:** A primary view listing a user's servers, displaying their specifications, IP address, and current status.
    *   **Live Status Updates:** The UI must provide feedback on long-running operations, for example by polling the API for status updates on servers that are `provisioning` or `rebooting`.
    *   **Server Actions:** Users must be able to trigger lifecycle actions (e.g., "Reboot", "Shutdown", "Terminate") directly from the dashboard.

### Migration Utility (Rust)

A separate, standalone Rust binary will be created to demonstrate a safe and effective data migration process.

*   **Direct Database Extraction:** The utility will connect directly to the WHMCS MariaDB/MySQL database to extract user and server data.
*   **Core Transformation Logic:** The script's primary purpose is to prove it can migrate active users and their servers. The most critical task is to correctly extract and transform data from WHMCS's **Configurable Options and Custom Fields** into the new, structured PostgreSQL schema.
*   **Essential Safeguards:** To prove the migration strategy is viable and safe, the utility must include:
    *   **Idempotency:** The script must be safe to run multiple times without creating duplicate data.
    *   **A `--dry-run` flag:** This will print a summary of actions that would be taken, allowing for validation without committing any changes.

### Full-Stack Quality and Validation

To validate the success of this build, a test suite covering both the frontend and backend is required.

*   **Backend Testing:**
    *   **Unit Tests:** Primarily for the migration's transformation logic, ensuring the mapping from WHMCS fields is correct.
    *   **Integration Tests:** To verify the core API endpoints, including authentication, error handling, and successful orchestration of a server creation request.
*   **Frontend Testing:**
    *   **Component Tests:** Basic tests (e.g., using Vitest/React Testing Library) must be included to verify the behavior of critical UI components like the login form and the server dashboard display.
