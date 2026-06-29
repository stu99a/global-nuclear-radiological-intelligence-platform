# Source Assessment & Data Profiling

**Project:** Global Nuclear & Radiological Intelligence Platform

---

# Document Information

| Item              | Value                                                             |
| ----------------- | ----------------------------------------------------------------- |
| Document          | Source Assessment & Data Profiling                                |
| Document ID       | GNRIP-003                                                         |
| Version           | 0.1                                                               |
| Status            | Draft                                                             |
| Author            | Josiah C. Mathew                                                  |
| Last Updated      | YYYY-MM-DD                                                        |
| Related Documents | GNRIP-001 Project Charter, GNRIP-002 Project Design Specification |

---

# Revision History

| Version | Date       | Description   | Author           |
| ------- | ---------- | ------------- | ---------------- |
| 0.1     | YYYY-MM-DD | Initial Draft | Josiah C. Mathew |

---

# 1. Purpose

This document evaluates the suitability of publicly available information sources for integration into the Global Nuclear & Radiological Intelligence Platform.

Each source is assessed against a common set of technical and analytical criteria to ensure it provides reliable, relevant, and maintainable data for intelligence analysis.

The assessment also profiles the structure and characteristics of each source to support downstream database design, ETL development, and analytical workflows.

---

# 2. Source Assessment Methodology

Each data source will be evaluated using a standardized assessment framework to ensure consistency across current and future integrations.

The assessment consists of two complementary stages:

1. **Source Assessment** – evaluates the suitability of the source from an architectural, legal, and analytical perspective.

2. **Data Profiling** – evaluates the structure, quality, and usability of the data contained within the source.

This methodology ensures that database design decisions are supported by measurable evidence rather than assumptions.

Each source is evaluated using the following criteria.

| Criterion            | Description                                                       |
| -------------------- | ----------------------------------------------------------------- |
| Authority            | Is the source published by a recognized and trusted organization? |
| Accessibility        | Is the information publicly available without authentication?     |
| Collection Method    | HTML, PDF, RSS, API, XML, etc.                                    |
| Update Frequency     | How frequently is new information published?                      |
| Data Quality         | Completeness and consistency of published information.            |
| Analytical Value     | Relevance to nuclear and radiological intelligence.               |
| Maintainability      | Expected stability of the source structure.                       |
| Legal Considerations | Collection permitted under publicly available website terms.      |

---

# 3. Version 1 Data Sources

Version 1 focuses on authoritative organizations with stable publication practices.

| Source                  | Priority | Status   |
| ----------------------- | -------- | -------- |
| IAEA News               | High     | Included |
| IAEA Press Releases     | High     | Included |
| IAEA Statements         | High     | Included |
| NRC Event Notifications | High     | Included |

Future versions may incorporate:

* INES / NEWS
* Nuclear Threat Initiative (NTI)
* U.S. Department of Energy (DOE)
* World Institute for Nuclear Security (WINS)

---

# 4. Source Profile — International Atomic Energy Agency (IAEA)

Each source will be profiled using a representative sample of published documents.

The objective is to determine which fields are consistently available, how reliably they can be extracted, and whether they provide analytical value to the platform.

Each candidate field will be evaluated using the following metrics.

| Metric | Description |
|---------|-------------|
| Availability | Percentage of sampled documents containing the field. |
| Consistency | Degree to which the field follows a consistent format or structure. |
| Completeness | Degree to which the extracted value is complete and meaningful. |
| Extractability | Difficulty of extracting the field using automated methods. |
| Analytical Value | Contribution of the field to intelligence analysis and reporting. |

The results of this assessment will guide the design of the operational database and Data Dictionary.

## Field Classification Framework

Following data profiling, fields will be classified according to observed availability.

| Availability | Classification |
|--------------|----------------|
| 95–100% | Mandatory |
| 70–94% | Expected |
| 30–69% | Optional |
| Below 30% | Rare |

Field classification provides guidance for schema design and ETL validation but does not automatically determine database constraints. Final implementation decisions will also consider analytical value and system requirements.

## Overview

The International Atomic Energy Agency publishes a wide range of information relating to nuclear safety, nuclear security, safeguards, technical cooperation, emergency preparedness, scientific research, and regulatory activities.

Version 1 will focus on text-based publications.

---

### Publication Types

* News
* Press Releases
* Statements

Future consideration:

* Videos
* Photo Essays

---

### Collection Method

HTML

---

### Observed Data Elements

* Title
* Publication Date
* Summary
* Article Body
* URL
* Publication Type
* Related Topics
* Images (optional)

---

### Strengths

* Highly authoritative
* Consistent publication structure
* Rich analytical content
* Global coverage
* Well-structured HTML

---

### Limitations

* Not every article contains structured metadata.
* Geographic information is embedded within article text.
* Topics require classification rather than direct extraction.

---

### Overall Assessment

**Suitable for Version 1**

---

# 5. Source Profile — U.S. Nuclear Regulatory Commission (NRC)

## Overview

The U.S. Nuclear Regulatory Commission publishes official event notifications, regulatory announcements, and public communications relating to nuclear facilities and regulated activities.

Version 1 focuses on Event Notification documents.

---

### Publication Types

* Event Notifications (PDF)

---

### Collection Method

PDF Extraction

---

### Expected Data Elements

* Event Number
* Event Date
* Facility
* State
* Description
* Event Status
* URL

---

### Strengths

* Official regulatory information
* Structured event reporting
* High operational relevance

---

### Limitations

* Published primarily as PDF documents.
* PDF formatting may change over time.
* Text extraction requires additional processing.

---

### Overall Assessment

**Suitable for Version 1**

---

# 6. Comparative Source Assessment

| Characteristic       | IAEA            | NRC             |
| -------------------- | --------------- | --------------- |
| Authority            | Excellent       | Excellent       |
| Collection Method    | HTML            | PDF             |
| Data Structure       | Semi-structured | Semi-structured |
| Update Frequency     | High            | High            |
| Analytical Value     | High            | High            |
| Global Coverage      | Yes             | Primarily U.S.  |
| Technical Complexity | Low             | Medium          |

---

# 7. Data Profiling Findings

The initial assessment indicates that Version 1 will require support for multiple document formats.

| Format | Source |
| ------ | ------ |
| HTML   | IAEA   |
| PDF    | NRC    |

Key observations:

* Document structure differs significantly between sources.
* Common metadata (title, publication date, URL) can be standardized.
* Additional information such as countries, organizations, facilities, and materials will often require extraction from document content rather than direct scraping.
* A modular collection architecture is appropriate due to differing publication formats.

---

# 8. Source Selection Decisions

The following decisions have been approved for Version 1.

| Decision                        | Rationale                                                         |
| ------------------------------- | ----------------------------------------------------------------- |
| Include IAEA News               | High-quality strategic intelligence.                              |
| Include IAEA Press Releases     | Official organizational communications.                           |
| Include IAEA Statements         | Policy and leadership perspectives.                               |
| Include NRC Event Notifications | Operational nuclear event reporting.                              |
| Exclude Videos                  | Limited structured textual information.                           |
| Exclude Photo Essays            | Primarily visual content with limited analytical value.           |
| Postpone INES                   | Reserved for Version 2 after Version 1 architecture is validated. |

# Evidence-Based Design Decisions

The source assessment and data profiling activities directly support the subsequent design artifacts.

Specifically:

- The Data Dictionary will use profiling results to determine mandatory, expected, optional, and derived fields.
- The Entity Relationship Diagram (ERD) will model only validated entities and relationships.
- The SQL Schema will implement constraints based on measured field availability and analytical importance.
- ETL validation rules will be derived from observed source characteristics rather than assumptions.
- 
---

# 9. Conclusions

The assessed sources satisfy the project's requirements for authority, accessibility, analytical value, and maintainability.

The combination of HTML-based publications from the IAEA and PDF-based event notifications from the NRC provides sufficient diversity to validate the modular collection architecture while remaining manageable for Version 1 implementation.

The assessment also confirms the need for standardized metadata extraction, normalized database design, and source-specific collection modules.

---

# Document Approval

**Prepared By**

Josiah C. Mathew

**Approval Status**

Draft
