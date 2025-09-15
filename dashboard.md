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
### Extended Technology & Protocol Stack

This stack includes the technologies explicitly mentioned and those implicitly required to successfully complete the project.

**Core Application & Language**
*   **Rust:** The primary programming language for the API and migration utility.
*   **Tokio:** The asynchronous runtime powering the `axum` web framework.
*   **Axum:** The web framework for building the core REST API.
*   **SQLx:** The asynchronous SQL toolkit for Rust, used for interacting with PostgreSQL.
*   **Diesel:** An alternative ORM and query builder for Rust (choice between `sqlx` and `diesel`).
*   **Serde:** The framework for serializing and deserializing Rust data structures, primarily for JSON API payloads.
*   **Clap:** A command-line argument parser for building the standalone migration utility.
*   **Tracing:** A framework for instrumenting Rust programs to collect structured, event-based diagnostic information.

**Database Systems**
*   **PostgreSQL:** The target database for the new, modern application.
*   **MariaDB/MySQL:** The source database used by the legacy WHMCS instance.
*   **SQL:** Proficiency in both PostgreSQL and MySQL dialects is required.

**Web & API Technologies**
*   **RESTful API Design:** Principles for creating clean, predictable web APIs.
*   **HTTP:** Core understanding of methods (GET, POST), status codes (e.g., 200 OK, 202 Accepted, 401 Unauthorized), and headers.
*   **JSON (JavaScript Object Notation):** The data format for API communication.
*   **JWT (JSON Web Tokens):** The standard for stateless API authentication. Crates like `jsonwebtoken` or `biscuit` would be used.

**Legacy System & Environment**
*   **WHMCS:** The legacy PHP-based billing and automation platform.
*   **PHP:** Basic understanding is helpful for navigating the WHMCS environment.
*   **LAMP Stack (Linux, Apache, MySQL, PHP):** The typical environment for running WHMCS.

**Development, Tooling & Design Patterns**
*   **Docker & Docker Compose:** For creating a reproducible local development environment that includes the WHMCS testbed, PostgreSQL, and the new Rust API.
*   **Cargo:** Rust's build system and package manager.
*   **Git & GitHub/GitLab:** For version control.
*   **`sqlx-cli`:** A command-line utility for managing database migrations (creating, applying, reverting).
*   **Mocking:** Using traits and dependency injection to mock external services (`ProxmoxClient`) for testing. Crates like `mockall` are common.
*   **Background Worker Pattern:** A design pattern for offloading long-running tasks (like server provisioning) from the main API request-response cycle.

---

### Required Skill Set

This translates the technology stack into the practical skills needed to execute the project.

**1. Rust Backend Development**
*   **Web API Construction:** Proficiency in building robust, asynchronous REST APIs using `axum`, including routing, state management, middleware, and error handling.
*   **Database Interaction:** Deep knowledge of using `sqlx` or `diesel` for type-safe, asynchronous database queries, transactions, and connection pooling.
*   **Authentication & Authorization:** Skill in implementing JWT-based authentication flows, including token generation, validation, and protecting endpoints.
*   **Asynchronous Processing:** Ability to design and implement background job processing within an application, using `tokio` tasks to handle long-running operations without blocking API responses.
*   **CLI Application Development:** Experience building sophisticated command-line tools in Rust using a library like `clap` to handle arguments, flags, and subcommands.

**2. Database Engineering & Data Migration**
*   **Relational Database Modeling:** The ability to design a clean, normalized PostgreSQL schema that effectively models the domain (users, servers, specs) and improves upon the legacy WHMCS structure.
*   **Complex Data Transformation:** The critical skill of analyzing a legacy database schema (WHMCS) and writing transformation logic to map its data—especially complex, unstructured data from "Custom Fields"—into a new, structured schema.
*   *   **ETL (Extract, Transform, Load) Principles:** Understanding the core concepts of data extraction from a source (MySQL), in-memory transformation (in Rust), and loading into a target (PostgreSQL).
*   **SQL Proficiency:** Strong SQL skills in both MySQL and PostgreSQL to query the source data effectively and verify the migrated data in the target.
*   **Idempotent Scripting:** The ability to write scripts that can be run multiple times with the same outcome, preventing data duplication or corruption.

**3. DevOps & Systems Administration**
*   **Containerization:** Expertise in using Docker and Docker Compose to define, build, and run a multi-service local development environment.
*   **Legacy System Management:** The practical ability to install, configure, and populate a PHP/MySQL application like WHMCS to serve as a realistic testbed.
*   **Database Migration Management:** Proficiency with tools like `sqlx-cli` to manage the evolution of the database schema in a controlled, versioned manner.

**4. Software Architecture & Quality Assurance**
*   **Test-Driven Development (TDD):** A strong discipline for writing unit tests for critical business logic (especially the data transformation rules) and integration tests for API endpoints.
*   **Mocking & Dependency Injection:** The ability to architect code using traits to decouple components, allowing for external services (like a hypervisor API) to be mocked during testing.
*   **Structured Logging:** Skill in using the `tracing` library to produce meaningful, structured logs that are essential for debugging a distributed or asynchronous system.

---

### Short Learning Roadmap

This roadmap provides a logical progression for an individual or team to acquire the skills and complete the project.

#### Phase 1: Environment and Foundations

*   **Goal:** Set up the complete development and testing environment.
*   **Steps:**
    1.  **Build the WHMCS Testbed:** The absolute first step. Use Docker Compose to create a service for WHMCS (using an existing Docker image) and a MariaDB database. Learn how to install, configure, and populate it with the required complex sample data (users, servers, custom fields). *This is your "source of truth."*
    2.  **Rust & Web API Basics:** If new to the stack, learn the fundamentals of Rust. Then, focus on `axum`. Build a simple "Hello World" API with a few endpoints.
    3.  **Database Setup:** Add a PostgreSQL service to your Docker Compose file. Learn `sqlx-cli` and create your initial database migrations for the `users` and `servers` tables in the new system.
    4.  **Connect API to DB:** Connect your `axum` application to the PostgreSQL database using `sqlx`. Implement a basic, unauthenticated endpoint to create and retrieve a user.

#### Phase 2: Core API Feature Development

*   **Goal:** Build out the functional, secure core of the new server management API.
*   **Steps:**
    1.  **Implement JWT Authentication:** Add a `/login` endpoint. Implement the logic to verify a user's credentials against the database and issue a JWT. Create middleware to protect all other endpoints, requiring a valid token.
    2.  **Design the Mocking Interface:** Define a `ProxmoxClient` (or similar) trait with methods like `provision_server`. Create a `MockProxmoxClient` implementation that simulates delays and potential errors for testing.
    3.  **Implement Asynchronous Provisioning:** Create the `POST /servers` endpoint. It should immediately return a `202 Accepted` response. Use `tokio::spawn` to launch a background task that calls the (mocked) provisioning client. Log the outcome of the background task.
    4.  **Write API Integration Tests:** Write tests that simulate a full user flow: register (or have a user in the DB), log in to get a token, and use that token to successfully request a new server. Assert that the initial response is `202`.

#### Phase 3: The Migration Utility

*   **Goal:** Develop and test the standalone tool for migrating data from the legacy system.
*   **Steps:**
    1.  **Build the CLI Skeleton:** Create a new binary project in Rust. Use `clap` to set up the command-line interface, including the essential `--dry-run` flag.
    2.  **Connect to Both Databases:** Write the logic for your utility to connect to the source WHMCS MariaDB database and the target PostgreSQL database.
    3.  **Implement the Transformation Logic (Test First!):** This is the most critical part. Write **unit tests** *before* you write the transformation code. Create sample WHMCS data structures in your tests and assert that your transformation functions correctly map them to the new schema, especially for "Configurable Options" and "Custom Fields."
    4.  **Build the Full Migration Flow:** Implement the core logic: Extract data from WHMCS, transform it in memory, and check for existing records in PostgreSQL (for idempotency). Based on the `--dry-run` flag, either print the intended actions or execute the `INSERT` statements into the new database.

#### Phase 4: Final Integration and Validation

*   **Goal:** Prove that the entire system works end-to-end.
*   **Steps:**
    1.  **Execute the Full Migration:** Run the compiled migration utility against your WHMCS testbed to populate the new PostgreSQL database.
    2.  **Verify with the API:** Manually (using a tool like `curl` or Postman) and programmatically (with integration tests), verify that you can use the API to interact with the migrated data. For example, log in as a user from WHMCS and fetch the details of their migrated server.
    3.  **Refine and Document:** Clean up the code, add comments, and write a `README.md` that explains how to set up the environment, run the API, and execute the migration.
