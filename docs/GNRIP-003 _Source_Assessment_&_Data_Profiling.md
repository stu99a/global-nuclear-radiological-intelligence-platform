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

# 2. Source Evaluation Criteria

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

### Expected Data Elements

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

# 7. Data Profiling Summary

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
