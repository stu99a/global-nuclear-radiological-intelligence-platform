# Data Dictionary

**Project:** Global Nuclear & Radiological Intelligence Platform

## Scope

This document defines the logical data model for the Global Nuclear & Radiological Intelligence Platform (GNRIP).

It specifies the entities, attributes, relationships, and business rules required to support Version 1 of the platform. Physical database implementation details, indexing strategies, SQL data types, and ETL implementation are intentionally excluded and are documented separately within subsequent design specifications.

---

# Document Information

| Item | Value |
|------|-------|
| Document | Data Dictionary |
| Document ID | GNRIP-004 |
| Version | 0.1 |
| Status | Approved |
| Author | Josiah C. Mathew |
| Last Updated | 2026-07-01 |
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
- Organization Type
- Facility Type
- Material Class
- File Format

Reference Data promotes consistency, validation, and analytical reporting.

---

## Operational Data

Operational Data stores metadata describing the collection, validation, processing, and maintenance of intelligence information.

Version 1 currently includes:

- Collection Log

Future versions may introduce additional operational entities such as:

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

## 4.1 Core Domain Entities
---
## 4.1.1 Source

## Purpose

Represents an authoritative information source from which documents are collected.

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

If additional source classifications become necessary, Source Type may be normalized into a dedicated Reference Data entity.

---

## Approval Status

**Approved – Version 1**

---

## 4.1.2 Document

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
- Every document shall originate from exactly one publication type.
- A document may describe zero, one, or many operational events.
- Multiple documents may describe the same operational event.
- A document may belong to multiple analytical categories.
- Only immutable business facts shall be stored within this entity.
- Collection and processing metadata shall be stored within operational entities.

---

## Approval Status

**Approved – Version 1**

---

## 4.1.3 Event

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
| status | TEXT | No | Current operational status of the event |
| description | TEXT | No | Standardized description of the operational occurrence |

If multinational operational events become common, the relationship may be refactored into an Event_Country associative entity.
---

## Relationships

Document (Many) --------< Document_Event >-------- Event (Many)

Event Type (1) --------< Event (Many)

Country (1) --------< Event (Many)

Event (Many) --------< Event_Facility >-------- Facility (Many)

Event (Many) --------< Event_Material >-------- Material (Many)

---

## Business Rules

- Every Event represents a single operational occurrence.
- Every Event shall belong to exactly one Event Type.
- Multiple Documents may describe the same Event.
- A single Document may describe zero, one, or many Events.
- An Event may involve one or more Facilities.
- A Facility may be associated with multiple Events.
- An Event may involve one or more Materials.
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
- Security exercise conducted across one or more nuclear installations.

---

### Future Considerations

If future sources provide structured lifecycle information for operational events, the current `status` attribute may be migrated to a dedicated `Event History` entity to preserve temporal changes without altering the core Event model.

---

## Approval Status

**Approved – Version 1**

---

## 4.2 Reference Data

Reference entities provide controlled vocabularies used throughout the operational data model.

Unlike core domain entities, reference entities contain standardized values that support data consistency, validation, querying, and analytics.

Version 1 includes the following reference entities:

Version 1 includes the following Reference Data entities:

Version 1 includes the following Reference Data entities:

- Publication Type
- Event Type
- Category
- Organization Type
- Facility Type
- Material Class
- File Format

Additional reference entities may be introduced in future versions where justified by new source requirements.

---

## 4.2.1 Publication Type

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

## 4.2.2 Event Type

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

## 4.2.3 Category

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

## 4.2.4 Organization Type

## Purpose

Represents the classification of organizations referenced within the platform.

The Organization Type entity provides a controlled vocabulary that standardizes the classification of organizations involved in nuclear and radiological activities. It promotes consistency across documents, events, reporting, and analytical workflows.

Examples include:

- International Organization
- Regulatory Authority
- Nuclear Operator
- Government Agency
- Research Institution
- University
- Healthcare Institution
- Private Company

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources reference multiple categories of organizations that participate in regulatory oversight, nuclear operations, research, emergency response, healthcare, and international cooperation.

Standardizing these classifications improves consistency and analytical reporting.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| organization_type_id | INTEGER | No | Primary Key |
| organization_type_name | TEXT | No | Name of the organization type |

---

## Relationships

Organization Type (1) --------< Organization (Many)

---

## Business Rules

- Every Organization shall belong to exactly one Organization Type.
- Organization Types shall be maintained as a controlled vocabulary.
- Organization Type names shall be unique.
- New Organization Types may be introduced without requiring schema modifications.

---

## Approved Controlled Vocabulary (Version 1)

| ID | Organization Type |
|----|-------------------|
| 1 | International Organization |
| 2 | Regulatory Authority |
| 3 | Government Agency |
| 4 | Nuclear Operator |
| 5 | Research Institution |
| 6 | University |
| 7 | Healthcare Institution |
| 8 | Private Company |

---

## Approval Status

**Approved – Version 1**

---

## 4.2.5 Facility Type

## Purpose

Represents the classification of facilities referenced within the platform.

The Facility Type entity provides a controlled vocabulary that standardizes the classification of facilities involved in nuclear and radiological activities. It promotes consistency across documents, operational events, reporting, and analytical workflows.

Examples include:

- Nuclear Power Plant
- Research Reactor
- Fuel Fabrication Facility
- Radioactive Waste Storage Facility
- Nuclear Research Laboratory
- Uranium Processing Facility
- Nuclear Medicine Facility
- Enrichment Facility

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources reference multiple categories of facilities associated with nuclear energy, research, medical applications, waste management, and regulatory activities.

Standardizing these classifications improves data consistency and analytical reporting.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| facility_type_id | INTEGER | No | Primary Key |
| facility_type_name | TEXT | No | Name of the facility type |

---

## Relationships

Facility Type (1) --------< Facility (Many)

---

## Business Rules

- Every Facility shall belong to exactly one Facility Type.
- Facility Types shall be maintained as a controlled vocabulary.
- Facility Type names shall be unique.
- New Facility Types may be introduced without requiring schema modifications.

---

## Approved Controlled Vocabulary (Version 1)

| ID | Facility Type |
|----|---------------|
| 1 | Nuclear Power Plant |
| 2 | Research Reactor |
| 3 | Fuel Fabrication Facility |
| 4 | Radioactive Waste Storage Facility |
| 5 | Nuclear Research Laboratory |
| 6 | Uranium Processing Facility |
| 7 | Nuclear Medicine Facility |
| 8 | Enrichment Facility |

---

## Approval Status

**Approved – Version 1**

---

## 4.2.6 Material Class

## Purpose

Represents the scientific or regulatory classification of materials referenced within the platform.

The Material Class entity provides a controlled vocabulary that standardizes the classification of nuclear and radiological materials while remaining consistent with internationally recognized terminology.

Material Class identifies **what a material is**, rather than how it is used, packaged, transported, or encountered during an operational event.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling
- IAEA Nuclear Security Glossary (2020)
- IAEA Safety Glossary (2018)

Current and planned sources reference numerous materials used in nuclear energy, medicine, industry, research, safeguards, and emergency response.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| material_class_id | INTEGER | No | Primary Key |
| material_class_name | TEXT | No | Scientific or regulatory classification of the material |

---

## Relationships

Material Class (1) --------< Material (Many)

---

## Business Rules

- Every Material shall belong to exactly one Material Class.
- Material Classes shall be maintained as a controlled vocabulary.
- Material Class names shall be unique.
- Material Classes classify the intrinsic nature of a material.
- Operational context (e.g., sealed source, spent fuel) shall not be stored within this entity.

---

## Approved Controlled Vocabulary (Version 1)

| ID | Material Class |
|----|----------------|
| 1 | Radioisotope |
| 2 | Nuclear Material |
| 3 | Nuclear Fuel |

---

## Future Considerations

Should future analytical requirements justify additional scientific classifications, new Material Classes may be introduced without requiring schema modifications.

Operational descriptors such as **Sealed Source**, **Unsealed Source**, **Spent Fuel**, and **Radioactive Waste** shall only be introduced as separate concepts when supported by structured source data.

---

# 4.2.7 File Format

## Purpose

Represents the format of content collected from external information sources.

The File Format entity provides a controlled vocabulary that standardizes the formats processed by the platform during data collection. Standardizing file formats supports ETL workflows, validation, reporting, and future expansion to additional collection methods.

Examples include:

- HTML
- PDF
- RSS
- JSON
- XML

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources publish information in multiple formats, including HTML web pages and PDF documents. Future integrations may include RSS feeds, JSON APIs, and XML-based data services.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| file_format_id | INTEGER | No | Primary Key |
| file_format_name | TEXT | No | Name of the file format |

---

## Relationships

File Format (1) --------< Collection Log (Many)

---

## Business Rules

- Every Collection Log shall reference exactly one File Format.
- File Formats shall be maintained as a controlled vocabulary.
- File Format names shall be unique.
- New File Formats may be introduced without requiring schema modifications.

---

## Approved Controlled Vocabulary (Version 1)

| ID | File Format |
|----|-------------|
| 1 | HTML |
| 2 | PDF |
| 3 | RSS |
| 4 | JSON |
| 5 | XML |

---

## Approval Status

**Approved – Version 1**

---

## 4.3 Master Data

## Purpose

Master Data represents authoritative real-world entities that are shared across the platform.

Unlike Reference Data, Master Data is governed by internationally recognized standards where available and serves as the foundation for operational processing, analytical reporting, and integration with external datasets.

Version 1 includes the following Master Data entities:

- Country *(Approved – Version 1)*
- Organization *(Approved – Version 1)*
- Facility *(Approved – Version 1)*
- Material *(Approved – Version 1)*

---

## 4.3.1 Country

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

## 4.3.2 Organization

## Purpose

Represents organizations referenced within published documents or operational events.

Organizations include regulatory authorities, international organizations, nuclear operators, research institutions, government agencies, healthcare institutions, universities, and private companies relevant to the nuclear and radiological domain.

Organizations are maintained as Master Data to promote consistent identification and analysis across multiple documents and events.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources reference numerous organizations in addition to the publishing authority. These organizations participate in operational events, regulatory oversight, research, emergency response, and international cooperation.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| organization_id | INTEGER | No | Primary Key |
| organization_name | TEXT | No | Official organization name |
| organization_type_id | INTEGER | No | Foreign Key to Organization Type |
| country_id | INTEGER | Yes | Foreign Key to Country |
| website | TEXT | Yes | Official website |

---

## Relationships

Organization Type (1) --------< Organization (Many)

Country (1) --------< Organization (Many)

Document (Many) --------< Document_Organization >-------- Organization (Many)

Organization (1) --------< Facility (Many)

---

## Business Rules

- Organization names shall be unique.
- Every Organization shall belong to exactly one Organization Type.
- An Organization may be be referenced by multiple Documents.
- An Organization may own or operate multiple Facilities.
- Country association is optional where not applicable.

---

## Approval Status

**Approved – Version 1**

---

## 4.3.3 Facility 

## Purpose

Represents a physical installation or operational site referenced within published documents or operational events.

Facilities include nuclear power plants, research reactors, laboratories, waste storage facilities, enrichment facilities, medical facilities, and other installations relevant to the nuclear and radiological domain.

Facilities are maintained as Master Data to support consistent identification, geographical analysis, and operational reporting across multiple documents and events.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

Current Version 1 sources regularly reference facilities involved in operational events, regulatory oversight, emergency response, research, and international cooperation.

Future integrations such as INES and other international datasets are expected to expand facility coverage.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| facility_id | INTEGER | No | Primary Key |
| facility_name | TEXT | No | Official facility name |
| facility_type_id | INTEGER | No | Foreign Key to Facility Type |
| organization_id | INTEGER | Yes | Foreign Key to Organization |
| country_id | INTEGER | No | Foreign Key to Country |

---

## Relationships

Facility (Many) --------< Event_Facility >-------- Event (Many)

Organization (1) --------< Facility (Many)

Country (1) --------< Facility (Many)

Facility (1) --------< Event (Many)

---

## Business Rules

- Every Facility shall belong to exactly one Facility Type.
- Every Facility shall belong to exactly one Country.
- A Facility may belong to one Organization.
- Organization association may be NULL when ownership or operation is unknown.
- Facility names shall be unique within a Country.

---

## Approval Status

**Approved – Version 1**

---

## 4.3.4 Material

## Purpose

Represents specific nuclear and radiological materials referenced within published documents or operational events.

Materials are the fundamental substances discussed within the intelligence domain and are maintained as Master Data to support consistent identification, querying, and analysis.

Examples include:

- Uranium-235
- Uranium-238
- Plutonium-239
- Cesium-137
- Cobalt-60
- Iridium-192
- Americium-241
- MOX Fuel

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling
- IAEA Nuclear Security Glossary (2020)
- IAEA Safety Glossary (2018)

Current and planned sources reference a wide range of nuclear and radiological materials associated with safeguards, security, medicine, industry, emergency response, and research.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| material_id | INTEGER | No | Primary Key |
| material_name | TEXT | No | Official material name |
| material_class_id | INTEGER | No | Foreign Key to Material Class |

---

## Relationships

Material Class (1) --------< Material (Many)

Document (Many) --------< Document_Material >-------- Material (Many)

Event (Many) --------< Event_Material >-------- Material (Many)

---

## Business Rules

- Every Material shall belong to exactly one Material Class.
- Material names shall be unique.
- A Material may be referenced by multiple Documents.
- A Material may be associated with multiple Events.
- Materials represent substances rather than operational objects or packaging.

---

## Approval Status

**Approved – Version 1**

---

## 4.4 Operational Data

## Purpose

Operational Data represents metadata describing the collection, validation, processing, and maintenance of intelligence information.

Unlike Core Domain Entities and Master Data, Operational Data supports ETL workflows, auditing, traceability, and data governance without representing real-world intelligence concepts.

The following operational entities are included within the Version 1 logical data model:

**Collection Log (Approved – Version 1)**

---

## 4.4.1 Collection Log

## Purpose

Represents the operational record of a data collection activity performed against an external information source.

Collection Logs provide traceability for ETL operations and support auditing, troubleshooting, and data governance without storing business or intelligence data.

---

## Evidence

Derived from:

- GNRIP-003 Source Assessment & Data Profiling

The platform performs periodic collection of publications from multiple authoritative sources. Recording collection activities supports data integrity, operational monitoring, and reproducibility.

---

## Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| collection_id | INTEGER | No | Primary Key |
| source_id | INTEGER | No | Foreign Key to Source |
| collection_timestamp | DATETIME | No | Date and time of the collection run |
| documents_discovered | INTEGER | No | Number of documents identified |
| documents_collected | INTEGER | No | Number of documents successfully collected |
| documents_failed | INTEGER | No | Number of collection failures |
| file_format_id | INTEGER | No | Foreign Key to File Format |

---

## Relationships

Source (1) --------< Collection Log (Many)

File Format (1) --------< Collection Log (Many)

---

## Business Rules

- Every Collection Log shall reference exactly one Source.
- Collection Logs represent operational metadata and shall not contain intelligence data.
- Collection statistics shall describe a single execution of the collection process.
- Collection Logs shall be immutable once created.
- Every Collection Log shall reference exactly one File Format.

---

## Approval Status

**Approved – Version 1**

---

## 4.5 Relationship Tables

## Purpose

Relationship Tables implement many-to-many relationships between entities while preserving database normalization and referential integrity.

Unlike Core Domain, Master Data, and Reference Data entities, Relationship Tables do not represent standalone business concepts. Their sole purpose is to associate records that may legitimately have multiple relationships.

Relationship Tables contain only foreign keys unless future analytical requirements justify additional relationship-specific attributes.

---

## Design Principles

Relationship Tables shall adhere to the following principles:

- Composite Primary Keys shall be used unless a surrogate key is justified.
- Each foreign key shall reference an existing parent entity.
- Relationship Tables shall not duplicate business attributes stored elsewhere.
- Additional attributes shall only be introduced when they describe the relationship itself rather than either parent entity.

---

## 4.5.1 Document_Event

### Purpose

Associates published Documents with the operational Events they describe.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Foreign Key to Document |
| event_id | INTEGER | No | Foreign Key to Event |

### Business Rules

- Composite Primary Key: (document_id, event_id)
- A Document may reference multiple Events.
- An Event may be described by multiple Documents.

---

## 4.5.2 Document_Category

### Purpose

Associates Documents with one or more analytical Categories.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Foreign Key to Document |
| category_id | INTEGER | No | Foreign Key to Category |

### Business Rules

- Composite Primary Key: (document_id, category_id)
- A Document may belong to multiple Categories.
- A Category may classify multiple Documents.

---

## 4.5.3 Document_Country

### Purpose

Associates Documents with Countries referenced within their content.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Foreign Key to Document |
| country_id | INTEGER | No | Foreign Key to Country |

### Business Rules

- Composite Primary Key: (document_id, country_id)
- A Document may reference multiple Countries.
- A Country may appear in multiple Documents.

---

## 4.5.4 Document_Organization

### Purpose

Associates Documents with Organizations referenced within their content.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Foreign Key to Document |
| organization_id | INTEGER | No | Foreign Key to Organization |

### Business Rules

- Composite Primary Key: (document_id, organization_id)
- A Document may reference multiple Organizations.
- An Organization may appear in multiple Documents.

---

## 4.5.5 Document_Material

### Purpose

Associates Documents with Materials referenced within their content.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| document_id | INTEGER | No | Foreign Key to Document |
| material_id | INTEGER | No | Foreign Key to Material |

### Business Rules

- Composite Primary Key: (document_id, material_id)
- A Document may reference multiple Materials.
- A Material may appear in multiple Documents.

---

## 4.5.6 Event_Facility

### Purpose

Associates operational Events with the Facilities involved.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| event_id | INTEGER | No | Foreign Key to Event |
| facility_id | INTEGER | No | Foreign Key to Facility |

### Business Rules

- Composite Primary Key: (event_id, facility_id)
- An Event may involve multiple Facilities.
- A Facility may participate in multiple Events.

---

## 4.5.7 Event_Material

### Purpose

Associates operational Events with the Materials involved.

### Attributes

| Field | Type | Null | Description |
|------|------|------|-------------|
| event_id | INTEGER | No | Foreign Key to Event |
| material_id | INTEGER | No | Foreign Key to Material |

### Business Rules

- Composite Primary Key: (event_id, material_id)
- An Event may involve multiple Materials.
- A Material may be associated with multiple Events.

---

# 5. Design Principles

The GNRIP Data Dictionary has been developed in accordance with the project's architectural and data governance principles.

1. Architecture shall precede implementation.
2. Store facts; derive insights through analysis.
3. Apply the Three-Query Rule when evaluating every attribute.
4. Separate business data from operational metadata.
5. Normalize entities and relationships where supported by evidence.
6. Design for extensibility without introducing speculative schema.
7. Record architectural decisions before implementation.
8. Represent controlled vocabularies using normalized Reference Data entities rather than free-text attributes.
9. Align domain terminology with authoritative standards wherever applicable (e.g., ISO, IAEA).
10. Preserve referential integrity through explicit entity relationships and relationship tables.

---

# 6. Document Approval

| Item | Value |
|------|-------|
| Document | GNRIP-004 Data Dictionary |
| Version | 1.0 |
| Prepared By | Josiah C. Mathew |
| Reviewed By | — |
| Approval Status | Approved |
| Last Updated | 2026-07-01 |
