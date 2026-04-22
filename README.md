# Overview

Fiscyon is a SaaS backend for processing crypto transaction histories into structured, reviewable, and report-ready fiscal data. It is built to solve a common operational problem in crypto accounting: exchange exports are inconsistent, source-specific, and difficult to transform into reliable records for holdings analysis, cost-basis calculation, and tax reporting.

At a high level, the platform ingests transaction files from supported exchanges, standardizes them into an internal format, enriches them with pricing context, evaluates them through FIFO-based calculation flows, and produces tax-report outputs that can be reviewed and persisted.

# Who It Is For

This system is oriented toward firms or professionals who manage crypto tax and accounting workflows for multiple clients.

Typical users include:

- tax advisory teams handling client crypto histories
- accounting or operations teams reviewing imported exchange data
- back-office workflows that need auditable transaction processing
- products that require structured crypto tax reporting as a service layer

# What the SaaS Does

The SaaS provides a backend workflow for turning raw exchange exports into usable fiscal records.

Its main product capabilities include:

- importing transaction history files from supported exchanges
- validating source ownership and import consistency
- staging and reviewing imported data before final promotion when needed
- normalizing heterogeneous exchange rows into a common internal transaction layer
- generating diagnostics and warnings for incomplete or ambiguous data
- resolving historical pricing and FX context for valuation in EUR
- calculating holdings and FIFO-based realized results
- reconciling transfer activity across wallets or accounts
- generating tax report previews, PDFs, and persisted report revisions

# High-Level Backend Architecture

The backend is implemented as a feature-oriented Express + Prisma application.

The architecture separates the platform into distinct processing stages rather than a single monolithic flow. Import handling, staging/review, normalization, valuation, FIFO calculation, and reporting are modeled as separate backend domains. This keeps ingestion concerns isolated from fiscal computation and makes the system easier to evolve as supported sources and reporting rules expand.

Protected routes are company-scoped, which makes tenant isolation a core part of the backend design rather than an afterthought.

# Main Backend Features

## Authentication and Tenant Access

Handles user access, authenticated sessions, and tenant-scoped request context. This feature ensures that operational and fiscal data is resolved within the correct company boundary.

## Client and Company Management

Provides the business context around the processing workflow. It manages tenant-level company information and the client records under which imports, transactions, holdings, and reports are organized.

## Exchange Account Linking

Tracks exchange-specific account context associated with a client. It supports ownership validation and helps the backend determine whether imported source files match the expected exchange identity.

## Import Ingestion

Accepts source files from supported exchanges and turns them into backend-managed imports. This feature is responsible for file intake, parser selection, import lifecycle handling, raw persistence, and import-level warning generation.

## Source-Specific Parsing

Implements exchange-aware ingestion logic for the currently supported providers in the repository, including Binance and Coinbase. Its role is to interpret different file formats and convert them into a backend-compatible raw representation.

## Staging and Review

Introduces a controlled review layer for imports that should not go directly into the main transaction dataset. This feature stores staged rows, review metadata, and approval signals before promotion into durable raw transactions.

## AI-Assisted Import Analysis

Supports AI-assisted assessment of staged imports, including profiling, mapping proposals, and review-oriented output. In the current repository, this capability is integrated as part of the staging flow rather than as a separate external subsystem.

## Normalization

Transforms raw exchange rows into a standardized internal transaction layer suitable for downstream holdings, valuation, and FIFO workflows. It is responsible for reducing source-specific variability and giving the rest of the backend a more stable input model.

## Validation and Diagnostics

Generates warnings, review signals, and processing diagnostics around import quality, unsupported activity, incomplete source history, and normalization outcomes. This feature is important because the SaaS is designed to surface uncertainty rather than hide it.

## Pricing and FX Resolution

Provides historical valuation support for crypto assets and fiat conversion. It allows the backend to enrich transaction processing and reporting with EUR-based pricing context, using persisted price and FX data to support repeatable calculations.

## Holdings

Builds asset-level balance summaries from normalized transaction flows. This gives the SaaS an operational view of client inventory before or alongside fiscal calculations.

## FIFO Calculation

Runs the cost-basis and realized-result workflow used for fiscal analysis. The repository contains both an original and a beta FIFO path, showing that the system supports iterative evolution of the calculation engine without collapsing the overall product flow.

## Transfer Reconciliation

Links related transfer movements so that activity across wallets or exchanges can be interpreted more consistently during downstream fiscal processing. This reduces ambiguity in situations where inventory continuity depends on relating separate movements.

## Tax Report Generation

Builds high-level tax-report outputs from processed transaction data and FIFO results. This includes report previews, PDF generation, persisted revisions, and workflow progression for draft and approved reports.

## Auditability and Traceability

The backend is designed to preserve processing context across the full lifecycle of an import. Review stages, warnings, promotion steps, normalized outputs, reconciliation artifacts, and report revisions all contribute to a more auditable SaaS workflow.

## Operational Scripts and Data Maintenance

The repository includes supporting scripts for backfilling price data, synchronizing asset/provider information, migration tasks, and comparison utilities. These are not the primary product surface, but they support the reliability and maintainability of the SaaS backend over time.

# End-to-End SaaS Flow

The SaaS starts when a user uploads a transaction export for a client. The backend identifies the source type, parses the file, and records an import with associated warnings or review signals. Depending on the path, imported content may be staged first for review and later promoted into the main raw transaction layer.

Once the import is accepted, the backend normalizes source-specific rows into a standardized transaction representation. From there, the system can compute holdings, resolve historical pricing and FX context, reconcile related transfers, and run FIFO-based fiscal calculations. The resulting data is then used to generate report previews, downloadable PDFs, and persisted report revisions for later retrieval and approval.

# Technical Highlights

- Feature-oriented backend organization with clear separation between ingestion, normalization, valuation, calculation, and reporting
- Multi-tenant request handling integrated into the core application flow
- Support for heterogeneous exchange sources through source-specific adapters
- Review-aware ingestion path that does not assume all imported data should be trusted immediately
- Strong emphasis on diagnostics, warnings, and explicit processing visibility
- Historical pricing and FX support designed to make repeated fiscal calculations more practical
- Transfer-aware FIFO workflow rather than a simplistic isolated transaction calculator
- Report persistence with revision and workflow handling, which improves operational traceability
- Coexistence of legacy and beta calculation paths, indicating an intentional migration strategy

# Disclaimer

This README intentionally focuses on the SaaS at a feature and workflow level. It omits low-level implementation details, internal functions, database design, and sensitive processing logic by design.
# Media

## 1- Clients Page
<img width="2529" height="1298" alt="image" src="https://github.com/user-attachments/assets/a6279b97-db29-4436-a8c4-fadef820d792" />


## 2- Client Details Page
<img width="2535" height="1297" alt="image" src="https://github.com/user-attachments/assets/0bed3299-6239-4211-a292-e3039d3e0a15" />


## 3- Empty Imports Page
<img width="2531" height="1299" alt="image" src="https://github.com/user-attachments/assets/c83f8814-79d5-4dc1-8477-0017f6de4b60" />


## 4- Import Modal Page
<img width="489" height="625" alt="image" src="https://github.com/user-attachments/assets/7c698da2-37a6-4571-bd7a-4e20bc8dda0f" />


## 5- Import Page
<img width="2531" height="1300" alt="image" src="https://github.com/user-attachments/assets/772ad2b8-2419-4711-b173-4b21143fa4e9" />


## 6- FIFO Calculation Page
<img width="2532" height="856" alt="image" src="https://github.com/user-attachments/assets/546672e0-0e18-44c6-b70c-0ce630048354" />
<img width="1256" height="1238" alt="image" src="https://github.com/user-attachments/assets/727c3ad0-97fa-4ac9-a89a-31904732b3eb" />
<img width="1261" height="866" alt="image" src="https://github.com/user-attachments/assets/3c7abe9a-7d26-4816-b640-3710ff0a0e71" />

## 7- Tax Report Page
<img width="2531" height="1101" alt="image" src="https://github.com/user-attachments/assets/4aaefc56-8df0-46ca-b509-20f5b129fd31" />
<img width="734" height="1246" alt="image" src="https://github.com/user-attachments/assets/a277157f-a524-44bd-8e1b-9fe735b0d5ec" />


