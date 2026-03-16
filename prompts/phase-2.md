# Phase 2: Detailed Design

Strictly follow all rules in @CLAUDE.md.
Deliverables: `@docs/design.md` / `@docs/domain-design.md` / `@docs/db-schema.md`

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- All documents: English (framework names and technical terms may remain as-is)

## Preconditions (STRICT)
Confirm that `@docs/requirements.md` is approved before starting.
If unapproved, ambiguous, or inconsistent: STOP → ask the user.
Re-interpreting or adding requirements is prohibited.
If design decisions require requirement changes: STOP immediately.

## Role
Operate as Senior System Architect, DDD Expert, and Clean Architecture Specialist.
No implementation code of any kind.

## Execution Order (mandatory — no skipping)

**Stage 1** → Complete `@docs/design.md` and `@docs/domain-design.md`
**Stage 2** → Complete `@docs/db-schema.md` (may only start after Stage 1 is approved)
**Stage 3** → Cross-validate all three documents → final approval

## Stage 1: Architecture & Domain Design Interview

Ask ONE question at a time in the order below. Wait for each answer before continuing.

1. Architecture style decision
2. Layer structure and responsibilities of each layer
3. Dependency direction definition
4. Tech stack, key libraries, and versions
5. Ubiquitous language definition
6. Entity, value object, and aggregate definitions
7. Domain rules and invariants
8. UseCase definitions
9. Repository interfaces
10. Non-functional requirements and error handling strategy

### SubAgent Usage ①: After Stage 1 is complete
→ Launch `backend-architect` to verify:
  - Whether Clean Architecture dependency directions are correctly defined
  - Whether layer boundaries are unambiguous
  - Whether there are design issues specific to mobile apps
→ Present any detected issues to the user and resolve them by mutual agreement before continuing.

### SubAgent Usage ②: After Stage 1 is approved
→ Launch `feedback-synthesizer` to verify:
  - Consistency of Entities, UseCases, and Repositories in `domain-design.md`
  - Any gaps in coverage against `@docs/requirements.md`
→ If issues are found, present them and resolve them before moving to Stage 2.

## Stage 2: DB Schema Design Interview

May only start after `@docs/design.md` and `@docs/domain-design.md` are approved.
Ask ONE question at a time in the order below.

1. Confirm what needs to be persisted (refer to Entities in `domain-design.md`)
2. Table / collection definitions (one table at a time)
3. Relationships between tables (foreign keys, one-to-many, many-to-many)
4. Index design
5. Migration strategy

### DB Schema-Specific Rules
- Always cross-check table definitions against `@docs/domain-design.md`
- Modifying domain models to accommodate DB structure is prohibited
- If DB design requires domain model changes: STOP → explain to the user

### SubAgent Usage ③: After Stage 2 is complete
→ Launch `legal-compliance-checker` to verify:
  - Whether the personal data / privacy data storage policy complies with App Store and Google Play guidelines
  - Consistency between `@docs/requirements.md` Section 4 (legal policy) and the schema design
→ If issues are found, present them and resolve them by mutual agreement.

## Document Update Rules
- After each section is completed, present **only the changed sections** (full re-display is not required)
- If a contradiction with existing content is detected: STOP → explain → resolve → then update
- Silent changes are prohibited

## Document Templates (structure must not be changed)

### @docs/design.md
```
# Design Document
## 1. Architecture Overview
## 2. Layer Structure and Responsibilities
## 3. Dependency Direction
## 4. Tech Stack
## 5. Module Boundaries
## 6. Data Flow
## 7. Error Handling Strategy
## 8. State Management Policy
## 9. Non-Functional Requirements Coverage
## 10. Approval
```

### @docs/domain-design.md
```
# Domain Design Document
## 1. Ubiquitous Language
## 2. Entities
## 3. Value Objects
## 4. Aggregates
## 5. Domain Rules and Invariants
## 6. UseCase Definitions
## 7. Repository Interfaces
## 8. Domain Events
## 9. Approval
```

### @docs/db-schema.md
```
# DB Schema Design Document
## 1. Persistence Target List
| Table Name | Corresponding Domain Entity | Notes |

## 2. Table Definitions
### 2.x [Table Name]
| Field Name | Data Type | Constraints | Description |

## 3. Relationships Between Tables
### 3.x [Relationship Name]
- Type (one-to-many / many-to-many, etc.) · Foreign key / reference target

## 4. Indexes
| Table Name | Field Name | Type | Reason |

## 5. Migration Strategy
### 5.1 Version Control Policy
### 5.2 Schema Change Rules

## 6. Approval
```

## Completion Criteria (confirm all items before requesting approval)

**design.md**
- [ ] Architecture style is defined
- [ ] Layer structure and dependency direction are explicitly stated
- [ ] Tech stack, libraries, and versions are specified

**domain-design.md**
- [ ] Ubiquitous language is defined
- [ ] All Entities and value objects are defined
- [ ] All domain rules and invariants are defined
- [ ] All UseCases are defined
- [ ] Repository interfaces are defined

**db-schema.md**
- [ ] All tables with fields, data types, and constraints are defined
- [ ] Relationships between tables are defined
- [ ] Migration strategy is defined
- [ ] Consistency with `domain-design.md` is verified

**Overall**
- [ ] Consistency with `@docs/requirements.md` is verified
- [ ] `backend-architect` architecture review completed
- [ ] `feedback-synthesizer` domain consistency check completed
- [ ] `legal-compliance-checker` privacy data verification completed

## Start
First confirm the content of `@docs/requirements.md`,
then ask the user about the architecture style for this system.