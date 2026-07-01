# GNRIP-005
# Entity Relationship Diagram (ERD)

Version: 1.0 (Draft)

---

# 1. Scope

This document defines the Entity Relationship Diagram (ERD) for the Global Nuclear & Radiological Intelligence Platform (GNRIP).

The ERD is derived directly from the approved logical data model documented in **GNRIP-004 Data Dictionary** and serves as the authoritative visual representation of the platform's entities, relationships, primary keys, foreign keys, and cardinalities.

This document introduces no additional entities, attributes, relationships, or business rules beyond those approved within GNRIP-004.

---

# 2. Purpose

The purpose of this document is to provide a visual representation of the approved Version 1 logical data model.

The Entity Relationship Diagram supports:

- Database design validation
- SQL schema development
- ETL implementation
- Data governance
- Future platform expansion
- Technical documentation

---

# 3. Relationship Notation

The Entity Relationship Diagram uses **Crow's Foot notation** to represent relationships between entities.

The following conventions apply throughout this document:

| Relationship | Meaning |
|--------------|---------|
| 1 : 1 | One-to-One |
| 1 : M | One-to-Many |
| M : N | Many-to-Many (implemented using Relationship Tables) |

Many-to-many relationships shall always be resolved through associative entities (Relationship Tables).

Primary Keys (PK) and Foreign Keys (FK) shall be clearly identified within the Logical ERD.

---

# 4. Design Assumptions

The Entity Relationship Diagram is derived exclusively from the approved logical data model documented in **GNRIP-004 Data Dictionary**.

The following assumptions apply:

- The ERD represents the logical database design.
- Physical implementation details are intentionally excluded.
- Many-to-many relationships are implemented through associative entities.
- Reference Data entities implement controlled vocabularies.
- Operational metadata is separated from business data.
- The ERD shall remain consistent with GNRIP-004.

---

# 5. Entity Classification

The approved Version 1 architecture groups entities into five logical categories.

## Core Domain

- Source
- Document
- Event

---

## Reference Data

- Publication Type
- Event Type
- Category
- Organization Type
- Facility Type
- Material Class
- File Format

---

## Master Data

- Country
- Organization
- Facility
- Material

---

## Operational Data

- Collection Log

---

## Relationship Tables

- Document_Event
- Document_Category
- Document_Country
- Document_Organization
- Document_Material
- Event_Facility
- Event_Material

---

# 6. Entity Inventory

The approved Version 1 Entity Relationship Diagram contains the following entities.

| Entity Group | Number of Entities |
|--------------|------------------:|
| Core Domain | 3 |
| Reference Data | 7 |
| Master Data | 4 |
| Operational Data | 1 |
| Relationship Tables | 7 |
| **Total** | **22** |

The Entity Relationship Diagram shall represent all twenty-two approved entities.

---

# 7. Relationship Matrix

The following relationships are defined within the approved logical data model.

| Parent Entity | Child Entity | Cardinality | Relationship Type |
|---------------|--------------|-------------|-------------------|
| Source | Document | 1:M | Direct |
| Source | Collection Log | 1:M | Direct |
| Publication Type | Document | 1:M | Direct |
| Event Type | Event | 1:M | Direct |
| Organization Type | Organization | 1:M | Direct |
| Facility Type | Facility | 1:M | Direct |
| Material Class | Material | 1:M | Direct |
| File Format | Collection Log | 1:M | Direct |
| Country | Organization | 1:M | Direct |
| Country | Facility | 1:M | Direct |
| Country | Event | 1:M | Direct |
| Organization | Facility | 1:M | Direct |
| Document | Document_Event | 1:M | Associative |
| Event | Document_Event | 1:M | Associative |
| Document | Document_Category | 1:M | Associative |
| Category | Document_Category | 1:M | Associative |
| Document | Document_Country | 1:M | Associative |
| Country | Document_Country | 1:M | Associative |
| Document | Document_Organization | 1:M | Associative |
| Organization | Document_Organization | 1:M | Associative |
| Document | Document_Material | 1:M | Associative |
| Material | Document_Material | 1:M | Associative |
| Event | Event_Facility | 1:M | Associative |
| Facility | Event_Facility | 1:M | Associative |
| Event | Event_Material | 1:M | Associative |
| Material | Event_Material | 1:M | Associative |

---

# 8. ERD Validation Criteria

The Entity Relationship Diagram shall satisfy the following validation criteria.

- All approved entities from GNRIP-004 shall be represented.
- All approved relationships shall be represented.
- Primary Keys shall be identified.
- Foreign Keys shall be identified.
- Crow's Foot notation shall be used consistently.
- Every many-to-many relationship shall be implemented through an associative entity.
- No undocumented entities shall be introduced.
- No undocumented attributes shall be introduced.
- The ERD shall remain fully consistent with GNRIP-004.

---

# 9. Conceptual Entity Relationship Diagram

*To be completed.*

The Conceptual ERD presents the high-level business view of the platform and illustrates relationships between Core Domain, Master Data, Reference Data, Operational Data, and Relationship Tables without displaying individual attributes.

---

# 10. Logical Entity Relationship Diagram

*To be completed.*

The Logical ERD presents the complete Version 1 logical database model including:

- Entities
- Attributes
- Primary Keys
- Foreign Keys
- Cardinalities
- Relationship Tables

---

# 11. Design Validation

The completed Entity Relationship Diagram shall be validated against the approved logical data model documented in **GNRIP-004**.

Validation shall confirm that:

- Every approved entity is represented.
- Every approved attribute is represented.
- Every approved relationship is represented.
- Primary and Foreign Keys are correctly identified.
- Cardinalities are correctly represented.
- Relationship Tables correctly resolve all many-to-many relationships.
- No deviations from GNRIP-004 exist.

---

# 12. Document Approval

| Item | Value |
|------|-------|
| Document | GNRIP-005 Entity Relationship Diagram |
| Version | 1.0 (Draft) |
| Prepared By | Josiah C. Mathew |
| Reviewed By | — |
| Approval Status | Draft |
| Last Updated | *Update as required* |
