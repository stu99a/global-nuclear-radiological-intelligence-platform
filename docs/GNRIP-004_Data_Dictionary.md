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
- Technical Report (Future)
- Guidance Document (Future)

A document is **not** the operational event itself. Instead, it serves as the information product that may describe zero, one, or many real-world operational events.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Multiple publication types published by authoritative organizations share common metadata that can be standardized within a normalized data model.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Primary Key |
| source_id | INTEGER | No | Foreign Key to Source |
| publication_type_id | INTEGER | No | Foreign Key to Publication Type |
| title | TEXT | No | Document title |
| summary | TEXT | Yes | Document summary or abstract |
| body | TEXT | No | Full document text |
| publication_date | DATE | No | Official publication date |
| url | TEXT | No | Original document URL |
| language | TEXT | No | Publication language |
| checksum | TEXT | No | Hash used for duplicate detection |

---

## Relationships

Source (1) --------< Document (Many)

Publication Type (1) --------< Document (Many)

Document (Many) --------< Document_Event >-------- Event (Many)

Document (Many) --------< Document_Category >-------- Category (Many)

Document (Many) --------< Document_Organization >-------- Organization (Many)

Document (Many) --------< Document_Country >-------- Country (Many)

---

## Business Rules

- Every document shall originate from exactly one source.
- Every document shall belong to exactly one publication type.
- A document may describe zero, one, or many operational events.
- Multiple documents may describe the same operational event.
- A document may belong to multiple analytical categories.
- Only immutable business facts shall be stored within this entity.
- Collection and processing metadata shall be stored within operational entities.

---

## Approval Status

**Approved – Version 1**

---

# 4.3 Event

## Purpose

Represents a single real-world operational occurrence of intelligence significance.

An Event is independent of any individual publication and may be described by one or more Documents published by different authoritative organizations.

Each Event shall be classified using exactly one Event Type, while the associated Documents may be classified under one or more Categories.

---

## Evidence

Derived from:

- NRC Event Notifications
- IAEA Operational Reporting
- Future INES Integration

These sources describe operational occurrences that can be normalized into a common event model while remaining independent of the publications that report them.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| event_id | INTEGER | No | Primary Key |
| event_type_id | INTEGER | No | Foreign Key to Event Type |
| event_reference | TEXT | Yes | Official identifier assigned by the reporting authority (e.g., NRC Event Number, INES Event ID) |
| event_date | DATE | No | Date on which the event occurred |
| country_id | INTEGER | Yes | Foreign Key to Country |
| facility_id | INTEGER | Yes | Foreign Key to Facility |
| status | TEXT | No | Operational status of the event |
| description | TEXT | No | Standardized description of the operational occurrence |

---

## Relationships

Document (Many) --------< Document_Event >-------- Event (Many)

Event Type (1) --------< Event (Many)

Country (1) --------< Event (Many)

Facility (1) --------< Event (Many)

---

## Business Rules

- Every Event represents a single operational occurrence.
- Every Event shall belong to exactly one Event Type.
- Multiple Documents may describe the same Event.
- A single Document may describe zero, one, or many Events.
- Event Types classify **what occurred**, while Categories classify **the subject matter discussed within Documents**.
- Operational severity shall not be stored until a standardized severity framework (e.g., INES) is implemented.
- Collection and processing metadata shall not be stored within this entity.

---

## Examples

Examples of individual Events include:

- Unauthorized access to a nuclear facility.
- Theft of a radioactive source during transport.
- Reactor shutdown at a licensed facility.
- Emergency response activation following a radiological incident.
- Regulatory enforcement action issued against an operator.
- Security exercise conducted at a nuclear installation.

---

### Future Considerations

If future sources provide structured lifecycle information for operational events, the current `status` attribute may be migrated to a dedicated `Event History` entity to preserve temporal changes without altering the core Event model.

---

## Approval Status

**Approved – Version 1**

---

# 4.4 Publication Type

## Purpose

Represents the classification of a published information product.

The Publication Type entity provides a controlled vocabulary that standardizes the types of publications collected from authoritative sources. It ensures consistent classification across current and future data sources while preventing inconsistencies caused by free-text values.

Examples include:

- News
- Press Release
- Statement
- Event Notification
- Technical Report (Future)
- Guidance Document (Future)

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Version 1 sources publish multiple types of information products that require standardized classification.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| publication_type_id | INTEGER | No | Primary Key |
| publication_type_name | TEXT | No | Name of the publication type |

---

## Relationships

Publication Type (1) --------< Document (Many)

---

## Business Rules

- Every document shall belong to exactly one publication type.
- Publication type names shall be unique.
- Publication types shall be maintained as a controlled vocabulary.
- New publication types may be added without requiring schema modifications.

---

## Initial Controlled Vocabulary

| ID | Publication Type |
|----|------------------|
| 1 | News |
| 2 | Press Release |
| 3 | Statement |
| 4 | Event Notification |
| 5 | Technical Report *(Future)* |
| 6 | Guidance Document *(Future)* |

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
