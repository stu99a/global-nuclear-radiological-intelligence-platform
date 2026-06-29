# Project Design Specification

**Project:** Global Nuclear & Radiological Intelligence Platform

**Subtitle:**
*A modular intelligence platform for collecting, integrating, analyzing, and visualizing publicly available nuclear and radiological information from authoritative sources.*

---

# Document Information

| Item              | Value                        |
| ----------------- | ---------------------------- |
| Document          | Project Design Specification |
| Document ID       | GNRIP-002                    |
| Version           | 0.1                          |
| Status            | Draft                        |
| Author            | Josiah C. Mathew             |
| Last Updated      | YYYY-MM-DD                   |
| Related Documents | GNRIP-001 Project Charter    |

---

# Revision History

| Version | Date       | Description   | Author           |
| ------- | ---------- | ------------- | ---------------- |
| 0.1     | YYYY-MM-DD | Initial Draft | Josiah C. Mathew |

---

# 1. System Overview

The Global Nuclear & Radiological Intelligence Platform is a modular Extract, Transform and Load (ETL) and intelligence analytics system designed to collect, normalize, store, analyze, and visualize publicly available nuclear and radiological information from multiple authoritative organizations.

The platform is designed around a modular architecture that separates data collection, processing, storage, analytics, and reporting into independent components. This approach improves maintainability, scalability, and future integration of additional intelligence sources.

---

# 2. System Architecture

The platform follows a layered architecture.

```text
Authoritative Sources
        │
        ▼
 Data Collection
        │
        ▼
 Data Validation
        │
        ▼
 Cleaning & Normalization
        │
        ▼
 SQLite Database
        │
        ▼
 SQL Analytics
        │
        ▼
 NLP Pipeline
        │
        ▼
 Power BI Dashboard
        │
        ▼
 Intelligence Reports
```

Each layer has a clearly defined responsibility and communicates only with adjacent layers.

---

# 3. Architectural Principles

The platform is designed according to the following engineering principles.

* Modular data collectors
* Separation of concerns
* Normalized relational database design
* Reproducible ETL workflows
* Extensible system architecture
* Source-independent data processing
* Transparent data transformations
* Maintainable and reusable code
* Documentation-driven development

---

# 4. System Components

| Component  | Purpose                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------- |
| Collectors | Retrieve information from authoritative sources.                                             |
| Validation | Verify collected data before processing.                                                     |
| Processing | Clean, normalize, and standardize collected information.                                     |
| Database   | Store normalized operational data.                                                           |
| Analytics  | Execute SQL queries and statistical analysis.                                                |
| NLP        | Perform topic classification, keyword extraction, and entity recognition (future milestone). |
| Dashboard  | Present analytical insights through interactive visualizations.                              |
| Reporting  | Generate structured intelligence products and summaries.                                     |

---

# 5. Data Flow

The platform follows a structured ETL workflow.

```text
Source

↓

Collector

↓

Raw Data

↓

Validation

↓

Cleaning

↓

Normalization

↓

Feature Engineering

↓

Database

↓

Analytics

↓

Dashboard

↓

Intelligence Reports
```

Each stage performs a single responsibility before passing the processed data to the next stage.

---

# 6. Source Architecture

Version 1 will support the following authoritative sources.

| Source                  | Format | Collection Method |
| ----------------------- | ------ | ----------------- |
| IAEA News               | HTML   | BeautifulSoup     |
| IAEA Press Releases     | HTML   | BeautifulSoup     |
| IAEA Statements         | HTML   | BeautifulSoup     |
| NRC Event Notifications | PDF    | PDF Extraction    |

Future versions will incorporate:

* INES / NEWS
* Nuclear Threat Initiative (NTI)
* U.S. Department of Energy (DOE)
* World Institute for Nuclear Security (WINS)
* RSS feeds

---

# 7. Processing Pipeline

The processing pipeline consists of the following stages.

### Collection

Retrieve documents from authoritative public sources.

### Validation

Verify document accessibility, completeness, and supported format.

### Cleaning

Remove unnecessary formatting and normalize extracted text.

### Standardization

Apply controlled vocabularies and standardized naming conventions.

### Duplicate Detection

Identify duplicate documents using URL validation and content comparison.

### Loading

Insert validated records into the operational database.

### Analysis

Execute SQL queries, trend analysis, and future NLP processing.

---

# 8. Database Design Overview

The operational database will use a normalized relational model.

Key design characteristics include:

* Primary and foreign key relationships
* Lookup tables for controlled vocabularies
* Bridge tables for many-to-many relationships
* Separation of operational data from future analytical models
* Support for modular expansion as additional sources are integrated

Detailed table definitions are provided in the Data Dictionary and SQL Schema documents.

---

# 9. Technology Stack

| Layer                | Technology                                              |
| -------------------- | ------------------------------------------------------- |
| Programming Language | Python                                                  |
| HTML Collection      | Requests, BeautifulSoup                                 |
| PDF Extraction       | PyMuPDF / pdfplumber (evaluation during implementation) |
| Data Processing      | Pandas                                                  |
| Database             | SQLite                                                  |
| SQL                  | SQLite SQL                                              |
| Visualization        | Power BI                                                |
| Version Control      | Git & GitHub                                            |
| Documentation        | Markdown                                                |

---

# 10. Planned Repository Structure

```text
global-nuclear-radiological-intelligence-platform/

docs/
src/
data/
    raw/
    processed/
    database/

dashboard/
notebooks/
tests/
screenshots/

requirements.txt
README.md
LICENSE
```

The implementation will prioritize reusable Python modules within the `src/` directory, while notebooks will be reserved for exploration, validation, and demonstration.

---

# 11. Development Phases

| Phase   | Deliverable             |
| ------- | ----------------------- |
| Phase 1 | Architecture & Planning |
| Phase 2 | Data Collection         |
| Phase 3 | Data Engineering        |
| Phase 4 | Database Development    |
| Phase 5 | Analytics               |
| Phase 6 | Dashboard Development   |
| Phase 7 | Intelligence Reporting  |

Each phase concludes with a design review before progressing to the next stage.

---

# 12. Future Enhancements

Future versions of the platform may include:

* Additional intelligence sources
* Automated scheduling
* Named Entity Recognition (NER)
* Topic modelling
* Semantic search
* Knowledge graph exploration
* Web application interface
* Automated intelligence brief generation

---

# 13. Key Design Decisions

| Decision                             | Rationale                                                                                        |
| ------------------------------------ | ------------------------------------------------------------------------------------------------ |
| Modular collectors                   | Enables independent maintenance and expansion of individual sources.                             |
| Separate Document and Event entities | Strategic publications and operational events have different structures and analytical purposes. |
| SQLite for Version 1                 | Lightweight, portable, and appropriate for the expected data volume.                             |
| Normalized operational database      | Reduces redundancy and improves data integrity.                                                  |
| Architecture-first development       | Establishes a stable foundation before implementation begins.                                    |
| Documentation-driven engineering     | Ensures design decisions are traceable and reproducible throughout the project lifecycle.        |

---

# Document Approval

**Prepared By**

Josiah C. Mathew

**Approval Status**

Draft
