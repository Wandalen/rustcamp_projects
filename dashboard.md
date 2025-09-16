# Project: Full-Stack Proxmox Management Platform in Rust & React

### Objective

Build a complete WHMCS replacement: a Rust API orchestrating Proxmox VE, a React dashboard for server management, and a migration utility to transfer legacy data.

### Core Deliverables

*   **WHMCS Testbed:** Local legacy system simulation for migration testing
*   **Proxmox VE Testbed:** Local virtualization environment for API integration
*   **Rust API:** Server lifecycle orchestration with Proxmox integration
*   **React Dashboard:** Modern web interface for server management
*   **Migration Utility:** Command-line tool for WHMCS-to-PostgreSQL data transfer

### Environment Setup

*   **WHMCS Testbed:** Local instance with sample users, server products, and complex **Configurable Options** (RAM, CPU) and **Custom Fields** (OS templates, datacenter locations) for migration testing
*   **Proxmox VE Testbed:** Running node with cloneable VM templates (Cloud-Init Ubuntu) accessible from the Rust API

### Rust API (Axum + PostgreSQL)

*   **Architecture:** Axum web framework, PostgreSQL persistence (SQLx/Diesel), structured logging (tracing)
*   **Core Features:** JWT authentication, async server provisioning (202 Accepted), full lifecycle endpoints
*   **Proxmox Integration:** Direct API integration for VM cloning, lifecycle management, and resource orchestration

### React Dashboard (Vite SPA)

*   **Stack:** React with Vite, modern UI component library
*   **Features:** JWT authentication, server listing with specs/status, live status updates via polling, direct server lifecycle actions

### Migration Utility (Rust CLI)

*   **Data Extraction:** Direct connection to WHMCS MariaDB/MySQL for user and server data
*   **Transformation:** Convert WHMCS **Configurable Options** and **Custom Fields** to structured PostgreSQL schema
*   **Safety Features:** Idempotent execution and `--dry-run` flag for safe validation

### Full-Stack Quality and Validation

To validate the success of this build, a comprehensive test and benchmark suite covering both the frontend and backend is required.

*   **Backend Testing:**
    *   **Unit Tests:** Primarily for the migration's transformation logic, ensuring the mapping from WHMCS fields is correct.
    *   **Integration Tests:** To verify the core API endpoints, including authentication, error handling, and successful orchestration of a server creation request.
*   **Frontend Testing:**
    *   **Component Tests:** Basic tests (e.g., using Vitest/React Testing Library) must be included to verify the behavior of critical UI components like the login form and the server dashboard display.
*   **Professional Performance Benchmarking:**
    *   **API Performance:** JWT authentication throughput, database query speeds, async task processing latency, and memory usage profiling
    *   **Migration Performance:** WHMCS transformation speed, bulk insert rates, memory efficiency, and idempotency validation
    *   **Frontend Performance:** React component render times, API response handling, and bundle size optimization
    *   **End-to-End Validation:** Complete workflow timing, concurrent user load testing, and connection pool performance
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
*   **Professional Benchmarking:** Expertise in designing, implementing, and analyzing comprehensive performance benchmarks for API endpoints, database operations, and full-system workflows including statistical analysis of performance trends and regression detection.

---

### Short Learning Roadmap

This roadmap provides a logical progression with curated YouTube courses to acquire the necessary skills and complete the project within a constrained timeframe. The total watch time is approximately 10 hours.

#### **Phase 1: Environment and Foundations (Approx. 3.5 hours)**

*   **Goal:** Set up the complete development and testing environment.
*   **Courses:**
    *   **Docker & PostgreSQL:** Start with the fundamentals of containerization and setting up your database.
        *   [Docker Crash Course for Absolute Beginners (1h 18m)](https://www.youtube.com/watch?v=pg19Z8LL06w)
        *   [PostgreSQL Tutorial for Beginners (2h 48m, watch the first hour)](https://www.youtube.com/watch?v=sp_n0f_d2-M)
    *   **WHMCS Setup:** Get a basic understanding of installing and configuring the legacy system.
        *   [Getting Started with WHMCS: Installing WHMCS (4m)](https://www.youtube.com/watch?v=Guw941-2-4A)
        *   [How to Setup WHMCS (7m)](https://www.youtube.com/watch?v=yW8eay8h2E8)
    *   **Rust & Axum Basics:** A quick introduction to get your first web server running.
        *   [Rust Axum Full Course - Web Development (1h 19m, watch the first 30 minutes)](https://www.youtube.com/watch?v=XZtlD_m59sM)

#### **Phase 2: Core API Feature Development (Approx. 3 hours)**

*   **Goal:** Build out the functional, secure core of the new server management API.
*   **Courses:**
    *   **Rust, Axum & SQLx:** Dive deeper into building a production-ready API with database integration.
        *   [Rust Axum Production Coding (3h 36m, watch the first 1.5 hours covering Tracing, Config, and Model Layer)](https://www.youtube.com/watch?v=3cA_mk4vdWY)
    *   **JWT Authentication in Rust:** Learn how to secure your API endpoints.
        *   [JWT Authentication in Rust | A Step-by-Step Guide (17m)](https://www.youtube.com/watch?v=hswk_54aVjM)
        *   [Secure Authentication with JWT in AXUM Rust (24m)](https://www.youtube.com/watch?v=FBnBcYp0K_A)
    *   **Asynchronous Rust with Tokio:** Understand the fundamentals of async programming in Rust.
        *   [Getting started with Tokio (5m)](https://www.youtube.com/watch?v=20-9yV-b-8A)

#### **Phase 3: The Migration Utility (Approx. 2 hours)**

*   **Goal:** Develop and test the standalone tool for migrating data from the legacy system.
*   **Courses:**
    *   **Building CLIs in Rust with Clap:** Learn how to create robust command-line applications.
        *   [Build a Robust CLI App in Rust with Clap (25m)](https://www.youtube.com/watch?v=CqTh0c4bAUA)
        *   [Parse Rust CLI Args With Clap Rust Tutorial (13m)](https://www.youtube.com/watch?v=l_tHR-q64yQ)
    *   **Data Migration Concepts:** A brief overview of the principles behind data migration.
        *   [Learn Data Migration Basics in 15 Minutes (15m)](https://www.youtube.com/watch?v=i-vj0u3_8d4)
    *   **Connecting to Databases with SQLx:** Practical knowledge for your migration script.
        *   [SQLx in 12 minutes - Rust + Actix Web + PostgreSQL (12m)](https://www.youtube.com/watch?v=5rGkG72-mJA)
        *   [Learn Rust SQLX on Postgres (29m)](https://www.youtube.com/watch?v=Ynzpq5g1P4Y)

#### **Phase 4: Final Integration and Validation (Approx. 1.5 hours of review)**

*   **Goal:** Prove that the entire system works end-to-end.
*   **Action:** At this stage, you will apply the knowledge gained from the previous phases. Refer back to the relevant sections of the courses as needed to complete the final integration and write your tests. This phase is more about hands-on application than new learning. Review the latter parts of the "Rust Axum Production Coding" course for testing and validation best practices.
