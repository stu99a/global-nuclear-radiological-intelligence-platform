# Data Dictionary

**Project:** Global Nuclear & Radiological Intelligence Platform

---

# Document Information

| Item | Value |
|------|-------|
| Document | Data Dictionary |
| Document ID | GNRIP-004 |
| Version | 0.1 |
| Status | Draft |
| Author | Josiah C. Mathew |
| Last Updated | YYYY-MM-DD |
| Related Documents | GNRIP-001 Project Charter, GNRIP-002 Project Design Specification, GNRIP-003 Source Assessment & Data Profiling |

---

# Revision History

| Version | Date | Description | Author |
|----------|------|-------------|--------|
| 0.1 | YYYY-MM-DD | Initial Draft | Josiah C. Mathew |

---

# 1. Purpose

The Data Dictionary defines the logical entities, attributes, relationships, and business rules that form the operational data model for the Global Nuclear & Radiological Intelligence Platform.

The document serves as the authoritative reference for the Entity Relationship Diagram (ERD), SQL Schema, ETL implementation, and analytical models.

---

# 2. Data Architecture Overview

The platform separates its data model into two complementary layers.

## Domain Layer

The Domain Layer stores intelligence information representing real-world entities and relationships.

Examples include:

- Source
- Document
- Event
- Country
- Organization
- Facility
- Material
- Category

These entities model the intelligence domain and remain independent of implementation details.

---

## Operational Layer

The Operational Layer stores metadata describing the collection, validation, and processing of intelligence information.

Examples include:

- Collection Log
- ETL Processing
- Data Quality Metrics
- Processing Errors

Operational entities describe **how** data was collected rather than **what** exists in the intelligence domain.

This separation improves maintainability, extensibility, and long-term scalability.

---

# 3. Entity Review Methodology

Each entity included within the operational data model has been evaluated using evidence gathered during source assessment and data profiling.

Every attribute shall satisfy at least one of the following criteria:

- Supports analytical queries.
- Supports operational processing.
- Supports data integrity or traceability.

This engineering principle is referred to throughout the project as the **Three-Query Rule**.

Entities are approved individually and become part of the controlled design baseline. Changes after approval require documented architectural justification.

---

# 4. Approved Entities

---

# 4.1 Source

## Purpose

Represents an authoritative organization from which information is collected.

Examples include:

- International Atomic Energy Agency (IAEA)
- U.S. Nuclear Regulatory Commission (NRC)

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

The platform supports multiple authoritative organizations using different collection methods.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| source_id | INTEGER | No | Primary Key |
| source_name | TEXT | No | Full organization name |
| abbreviation | TEXT | No | Short identifier (e.g., IAEA, NRC) |
| source_type | TEXT | No | Organization classification |
| website | TEXT | No | Official website |

---

## Relationships

Source (1) --------< Document (Many)

Source (1) --------< Collection Log (Many)

---

## Business Rules

- Every document shall originate from exactly one source.
- A source may publish many documents.
- ETL implementation details shall not be stored within this entity.

---

## Approval Status

**Approved – Version 1**

---

# 4.2 Document

## Purpose

Represents a published information product collected from an authoritative source.

A document may represent:

- News
- Press Release
- Statement
- Event Notification
- Technical Report (future)
- Guidance Document (future)

A document is **not** the operational event itself.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Multiple publication types share common metadata suitable for normalization.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Primary Key |
| source_id | INTEGER | No | Foreign Key to Source |
| document_type_id | INTEGER | No | Foreign Key to Document Type |
| title | TEXT | No | Document title |
| summary | TEXT | Yes | Document summary |
| body | TEXT | No | Full document text |
| publication_date | DATE | No | Publication date |
| url | TEXT | No | Original document URL |
| language | TEXT | No | Publication language |
| checksum | TEXT | No | Duplicate detection checksum |

---

## Relationships

Document (Many) --------< Source (1)

Document (Many) --------< Document_Event >-------- Event (Many)

Document (Many) --------< Category (Many)

Document (Many) --------< Organization (Many)

Document (Many) --------< Country (Many)

---

## Business Rules

- Every document belongs to exactly one source.
- A document may describe zero, one, or many operational events.
- A document may belong to multiple analytical categories.
- Only immutable business facts shall be stored within this entity.
- ETL metadata shall be stored in operational entities.

---

## Approval Status

**Approved – Version 1**

---

# 4.3 Event

## Purpose

Represents a discrete operational occurrence of intelligence significance.

Examples include:

- Reactor shutdown
- Nuclear security incident
- Radioactive source theft
- Emergency response activation
- Regulatory enforcement action
- Security exercise
- INES incident (future)

Events represent real-world occurrences and are independent of any individual publication.

---

## Evidence

Derived from:

- NRC Event Notifications
- IAEA Operational Reporting
- Future INES Integration

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| event_id | INTEGER | No | Primary Key |
| event_type_id | INTEGER | No | Foreign Key to Event Type |
| event_reference | TEXT | Yes | Official event identifier |
| event_date | DATE | No | Date of occurrence |
| country_id | INTEGER | Yes | Foreign Key to Country |
| facility_id | INTEGER | Yes | Foreign Key to Facility |
| status | TEXT | No | Operational status |
| description | TEXT | No | Event description |

---

## Relationships

Event (Many) --------< Document_Event >-------- Document (Many)

Event (Many) --------< Country (1)

Event (Many) --------< Facility (1)

Event (Many) --------< Event Type (1)

---

## Business Rules

- Events represent operational occurrences, not publications.
- Multiple documents may describe the same event.
- A single document may describe multiple events.
- Severity shall not be stored until a standardized framework (e.g., INES) is adopted.

---

## Approval Status

**Approved – Version 1**

---

# 5. Pending Entities

The following entities remain under design review:

- Country
- Organization
- Facility
- Material
- Category
- Document Type
- Event Type
- Collection Log
- Document_Event (Bridge Table)
- Document_Category (Bridge Table)
- Document_Organization (Bridge Table)
- Document_Country (Bridge Table)

These entities will be reviewed individually before inclusion in the approved data model.

---

# 6. Design Principles

The Data Dictionary follows the engineering principles established for the project.

1. Architecture before implementation.
2. Store facts, derive insights.
3. Apply the Three-Query Rule to every attribute.
4. Separate domain data from operational metadata.
5. Normalize where supported by evidence.
6. Design for extensibility without speculative schema.
7. Document architectural decisions before implementation.

---

# 7. Document Approval

**Prepared By**

Josiah C. Mathew

**Approval Status**

Draft
