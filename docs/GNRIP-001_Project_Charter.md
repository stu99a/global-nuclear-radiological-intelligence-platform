# Project Charter

**Project:** Global Nuclear & Radiological Intelligence Platform

**Subtitle:**  
*A modular intelligence platform for collecting, integrating, analyzing, and visualizing publicly available nuclear and radiological information from authoritative sources.*

---

## Document Information

| Item | Value |
|------|-------|
| Document | Project Charter |
| Version | 0.1 |
| Status | Draft |
| Document ID | GNRIP-001 |
| Project Owner | Josiah C. Mathew |
| Repository | global-nuclear-radiological-intelligence-platform |
| Last Updated | YYYY-MM-DD |
| Next Review | Completion of Milestone 1 |

---

## Revision History

| Version | Date | Description | Author |
|----------|------|-------------|--------|
| 0.1 | YYYY-MM-DD | Initial Draft | Josiah C. Mathew |

---

# 1. Project Overview

The Global Nuclear & Radiological Intelligence Platform is a modular data engineering and intelligence analytics platform designed to collect, integrate, store, analyze, and visualize publicly available nuclear and radiological information from authoritative international and national organizations.

The platform aims to improve situational awareness by consolidating information from multiple sources into a standardized analytical environment. Through structured data collection, relational database design, SQL analytics, natural language processing (NLP), and interactive dashboards, the project demonstrates how open-source information can support intelligence analysis within the nuclear and radiological domain.

---

# 2. Background

Nuclear and radiological information is published by numerous organizations including international agencies, national regulators, research institutions, and security organizations.

Although these sources provide valuable information, they differ significantly in structure, publication format, and accessibility. Information may be distributed as HTML pages, PDF reports, RSS feeds, press releases, statements, or technical publications.

Analysts often spend considerable time manually gathering and organizing information before meaningful analysis can begin.

This project addresses that challenge by developing a unified intelligence platform capable of integrating multiple authoritative sources into a single analytical framework.

---

# 3. Problem Statement

Publicly available nuclear and radiological information is fragmented across multiple authoritative organizations. Differences in publication formats, document structures, and information models make longitudinal analysis and cross-source situational awareness difficult without significant manual effort.

The absence of a standardized analytical workflow limits the ability to efficiently identify trends, compare reporting across organizations, and generate intelligence products from open-source information.

---

# 4. Project Objectives

## Primary Objectives

- Collect information from multiple authoritative public sources.
- Standardize heterogeneous datasets into a common structure.
- Store normalized data in a relational database.
- Support SQL-based analytical queries.
- Produce interactive dashboards for intelligence analysis.
- Generate intelligence summaries from collected information.

---

## Secondary Objectives

- Demonstrate modern ETL practices.
- Showcase modular software architecture.
- Apply data governance principles.
- Provide a reusable framework for adding future intelligence sources.
- Build a portfolio-quality intelligence platform suitable for demonstrating data engineering and intelligence analysis skills.

---

# 5. Project Scope

## In Scope

### Data Collection

- IAEA News
- IAEA Press Releases
- IAEA Statements
- NRC Event Notifications

### Data Engineering

- HTML collection
- PDF extraction
- Data cleaning
- Data normalization
- Duplicate detection
- Data quality assessment

### Database

- SQLite
- Relational schema
- SQL queries
- Views

### Analytics

- Exploratory analysis
- Intelligence dashboards
- Trend analysis
- Topic classification (future milestone)
- Named Entity Recognition (future milestone)

---

## Out of Scope

The project will **not** include:

- Classified information
- Restricted databases
- Sensitive government systems
- Predictive threat assessment
- Attribution of malicious actors
- Operational decision support
- Live monitoring or alerting
- Vulnerability assessment
- Offensive cybersecurity capabilities

---

# 6. Stakeholders

The platform is designed to support users such as:

- Nuclear Security Analysts
- Nuclear Safety Analysts
- Policy Analysts
- Emergency Preparedness Officers
- Researchers
- Academic Institutions
- Students of Nuclear Security
- International Organizations

---

# 7. Success Criteria

The project will be considered successful when it can:

- Collect data from multiple authoritative sources.
- Normalize heterogeneous information into a unified schema.
- Store information within a relational database.
- Execute predefined intelligence-oriented SQL queries.
- Produce interactive analytical dashboards.
- Generate structured intelligence summaries.
- Support future expansion through modular collectors.

---

# 8. Constraints

The project will operate under the following constraints:

- Publicly available information only.
- Compliance with website terms of use.
- Respect for robots.txt where applicable.
- English-language sources during Version 1.
- SQLite as the initial database engine.
- Modular architecture supporting future expansion.

---

# 9. Risks and Mitigation

| Risk | Mitigation |
|------|------------|
| Website structure changes | Modular source-specific collectors |
| PDF layout changes | Independent PDF extraction module |
| Missing metadata | Data quality flags and validation |
| Duplicate documents | URL validation and checksum comparison |
| New document formats | Collector abstraction layer |
| Source unavailability | Independent ingestion pipelines |

---

# 10. Project Roadmap

## Version 1

- Project architecture
- Source assessment
- Data dictionary
- ERD
- SQL schema
- IAEA collector
- NRC collector
- SQLite database
- SQL analysis
- Initial dashboards

---

## Version 2

- INES integration
- NTI integration
- DOE integration
- WINS integration
- RSS ingestion
- Improved NLP

---

## Version 3

- Named Entity Recognition
- Topic modelling
- Semantic search
- Knowledge graph exploration
- Enhanced intelligence products

---

## Version 4

- Automated intelligence briefs
- LLM-assisted summarization (human reviewed)
- Web application
- Advanced analytics

---

# 11. Design Principles

The project will be developed according to the following engineering principles:

1. Architecture before implementation.
2. Store facts, derive insights.
3. Every field must satisfy the Three-Query Rule.
4. Prefer normalized operational models before analytical models.
5. Every architectural decision must be documented.
6. Use authoritative public sources.
7. Design for extensibility.
8. Prioritize maintainability over complexity.
9. Optimize for intelligence analysis rather than simple reporting.
10. Maintain reproducible and transparent data engineering workflows.

---

# 12. Non-Functional Requirements

The platform shall satisfy the following non-functional requirements throughout its development lifecycle.

| Requirement | Description |
|-------------|-------------|
| Maintainability | The system shall use a modular architecture with reusable collectors, processing modules, and database components to simplify maintenance and future enhancements. |
| Scalability | New data sources shall be added without requiring major changes to the database schema or ETL pipeline. |
| Reliability | Data collection failures shall be logged without terminating the entire collection process. |
| Reproducibility | Every collected record shall be traceable to its original source, collection date, and processing stage. |
| Transparency | Data transformations, cleaning procedures, and analytical assumptions shall be documented and reproducible. |
| Data Quality | Duplicate detection, validation rules, and quality checks shall be applied throughout the ETL process. |
| Performance | The platform shall efficiently process large collections of documents while supporting future expansion to additional data sources. |
| Extensibility | The architecture shall support the integration of additional organizations, document formats, NLP modules, and analytical capabilities without significant redesign. |
| Consistency | Naming conventions, controlled vocabularies, and data governance standards shall be applied across all project components. |
| Portability | The platform shall run on a standard Python environment with documented dependencies and without requiring proprietary software. |

---

# 13. Expected Deliverables

- Project Design Specification
- Source Assessment
- Data Dictionary
- Entity Relationship Diagram (ERD)
- SQL Schema
- ETL Pipeline
- SQLite Database
- SQL Analysis
- Intelligence Dashboards
- Intelligence Summary Reports
- Technical Documentation

---

# 14. Documaent Approval

This charter establishes the scope, objectives, governance principles, and development direction for the Global Nuclear & Radiological Intelligence Platform. All future architectural and implementation decisions should align with the objectives and design principles defined within this document.

Prepared By:
Josiah C. Mathew

Approval Status:
Draft
---
