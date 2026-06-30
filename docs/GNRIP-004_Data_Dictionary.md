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

A third group of entities, known as Reference Entities, provides controlled vocabularies that standardize classifications used throughout the domain model.

## Core Domain Entities

Core Domain Entities represent the primary intelligence concepts managed by the platform.

Examples include:

- Source
- Document
- Event

These entities model real-world intelligence information and remain independent of implementation details.

---

## Master Data

Master Data represents authoritative real-world entities that are shared across the platform.

Examples include:

- Country
- Organization
- Facility
- Material

Master Data follows recognized standards where available and supports interoperability across the platform.

---

## Reference Data

Reference Data provides controlled vocabularies used to standardize classifications throughout the platform.

Examples include:

- Publication Type
- Event Type
- Category

Reference Data promotes consistency, validation, and analytical reporting.

---

## Operational Data

Operational Data stores metadata describing the collection, validation, processing, and maintenance of intelligence information.

Examples include:

- Collection Log
- ETL Processing
- Data Quality Metrics
- Processing Errors

Operational Data describes **how** information is managed rather than **what** exists in the intelligence domain.

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

# 4. Data Model

# 4.1 Core Domain Entities

# 4.1.1 Source

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

# 4.1.2 Document

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

# 4.1.3 Event

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

# 4.2 Reference Entities

Reference entities provide controlled vocabularies used throughout the operational data model.

Unlike core domain entities, reference entities contain standardized values that support data consistency, validation, querying, and analytics.

Version 1 includes the following reference entities:

- Publication Type
- Event Type
- Category

Additional reference entities may be introduced in future versions where justified by new source requirements.

---

# 4.2.1 Publication Type

## Purpose

Represents the classification of a published information product.

The entity provides a controlled vocabulary that standardizes the types of publications collected from authoritative sources. It ensures consistent classification across current and future data sources while preventing inconsistencies caused by free-text values.

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

## Approved Controlled Vocabulary (Version 1)

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

# 4.2.2 Event Type

## Purpose

Represents the classification of an operational occurrence.

The entity provides a controlled vocabulary that standardizes the types of operational events recorded within the platform. It classifies **what happened** during an Event and supports consistent reporting, querying, and analytical workflows.

Examples include:

- Unauthorized Access
- Theft
- Smuggling
- Reactor Shutdown
- Radiation Exposure
- Emergency Response
- Security Exercise
- Regulatory Inspection
- Enforcement Action

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Version 1 operational sources (NRC Event Notifications and IAEA operational reporting) describe multiple classes of operational occurrences that require standardized classification.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| event_type_id | INTEGER | No | Primary Key |
| event_type_name | TEXT | No | Name of the event type |

---

## Relationships

Event Type (1)
        │
        ▼
Event (Many)

---

## Business Rules

- Every Event shall belong to exactly one Event Type.
- Event Types classify operational occurrences.
- Event Type names shall be unique.
- Event Types shall be maintained as a controlled vocabulary.
- New Event Types may be introduced without requiring schema modifications.

---

## Approved Controlled Vocabulary (Version 1)

| ID | Event Type |
|----|------------|
| 1 | Unauthorized Access |
| 2 | Theft |
| 3 | Smuggling |
| 4 | Reactor Shutdown |
| 5 | Radiation Exposure |
| 6 | Emergency Response |
| 7 | Security Exercise |
| 8 | Regulatory Inspection |
| 9 | Enforcement Action |
| 10 | Material Recovery |
| 11 | Material Loss |

---

## Approval Status

**Approved – Version 1**

---

# 4.2.3 Category

## Purpose

Represents the intelligence subject discussed within a published document.

The entity provides a controlled vocabulary for classifying documents according to their primary intelligence domains. Categories support document organization, filtering, trend analysis, dashboarding, reporting, and future Natural Language Processing (NLP) workflows.

Categories answer the question:

> **"What is this publication about?"**

Categories are independent of both Publication Type and Event Type.

A document may belong to one or more Categories.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources contain publications covering multiple intelligence domains, including nuclear security, nuclear safety, safeguards, emergency preparedness, policy, research, and technical cooperation.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| category_id | INTEGER | No | Primary Key |
| category_name | TEXT | No | Name of the category |
| parent_category_id | INTEGER | Yes | Self-referencing parent category |

---

## Relationships

Category (1) --------< Category (Many)

(Self-referencing hierarchy)

Document (Many) --------< Document_Category >-------- Category (Many)

---

## Business Rules

- Categories shall be maintained as a controlled vocabulary.
- Categories classify the intelligence subject discussed within a Document.
- Publication Types classify the type of publication.
- Event Types classify the type of operational occurrence.
- Categories may be organized hierarchically.
- Root Categories shall have a NULL parent_category_id.
- A Document may belong to one or more Categories.

---

## Future Considerations

The controlled vocabulary, hierarchical structure, governance, and maintenance of Categories will be documented separately in:

**GNRIP-009 – Intelligence Taxonomy**

Separating taxonomy governance from database structure allows the taxonomy to evolve independently of the database schema.

---

## Approval Status

**Approved – Version 1**

---

# 4.3 Master Data

## Purpose

Master Data represents authoritative real-world entities that are shared across the platform.

Unlike Reference Data, Master Data is governed by internationally recognized standards where available and serves as the foundation for operational processing, analytical reporting, and integration with external datasets.

The following Master Data entities are included within the Version 1 logical data model:

- Country *(Approved – Version 1)*
- Organization *(Under Design)*
- Facility *(Under Design)*
- Material *(Under Design)*

---

# 4.3.1 Country

## Purpose

Represents sovereign states and territories referenced within operational events and published documents.

The Country entity provides standardized geographic information that supports trend analysis, regional reporting, dashboard visualizations, and future integration with external datasets.

Countries are treated as **Master Data** rather than simple lookup values to promote interoperability and data consistency.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources regularly reference countries in relation to operational events, facilities, regulatory activities, emergency response, and international cooperation.

Future sources such as INES, NTI, DOE, and WINS are also expected to contain standardized country references.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| country_id | INTEGER | No | Primary Key |
| country_name | TEXT | No | Official country name |
| iso2_code | TEXT | No | ISO 3166-1 Alpha-2 code |
| iso3_code | TEXT | No | ISO 3166-1 Alpha-3 code |

---

## Relationships

Country (1) --------< Event (Many)

Country (Many) --------< Document_Country >-------- Document (Many)

Country (1) --------< Facility (Many)

---

## Business Rules

- Countries shall conform to the ISO 3166-1 international standard.
- Country names shall be unique.
- ISO Alpha-2 codes shall be unique.
- ISO Alpha-3 codes shall be unique.
- Every Facility shall belong to exactly one Country.
- Documents may reference one or more Countries.
- Events may reference one Country when applicable.

---

## Master Data Standard

The Country entity shall use the ISO 3166-1 standard as the authoritative reference for country names and country codes.

The dataset shall be maintained independently of operational data.

---

## Future Considerations

Should future analytical requirements include regional reporting, the Country entity may be extended with additional standardized attributes such as:

- UN Region
- IAEA Regional Office
- Geographic Coordinates (Country Centroid)

These additions shall only be introduced when supported by analytical requirements.

---

## Approval Status

**Approved – Version 1**

---

# 4.3.2 Organization

*Under Design*

---

# 4.3.3 Facility

*Under Design*

---

# 4.3.4 Material

*Under Design*

---

# 4.4 Operational Data

## Purpose

Operational Data represents metadata describing the collection, validation, processing, and maintenance of intelligence information.

Unlike Core Domain Entities and Master Data, Operational Data supports ETL workflows, auditing, traceability, and data governance without representing real-world intelligence concepts.

The following operational entities are included within the Version 1 logical data model:

- Collection Log *(Under Design)*

---

# 4.4.1 Collection Log

*Under Design*

---

# 4.5 Relationship Tables

## Purpose

Relationship Tables implement many-to-many relationships between entities while maintaining database normalization and referential integrity.

These tables contain relationship mappings only and do not represent standalone business concepts.

The following relationship tables are included within the Version 1 logical data model:

- Document_Event *(Under Design)*
- Document_Category *(Under Design)*
- Document_Country *(Under Design)*
- Document_Organization *(Under Design)*

Additional relationship tables may be introduced where justified by future analytical requirements.

---

# 4.5.1 Document_Event

*Under Design*

---

# 4.5.2 Document_Category

*Under Design*

---

# 4.5.3 Document_Country

*Under Design*

---

# 4.5.4 Document_Organization

*Under Design*

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
8. Controlled vocabularies shall be implemented using normalized reference entities rather than free-text attributes.

---

# 7. Document Approval

**Prepared By**

Josiah C. Mathew

**Approval Status**

Draft
