## Workflow Orchestration

### 1. Plan Node Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately – don't keep pushing
- Use plan mode for verification steps, not just building
- Required specifications must be clearly defined before any implementation.
- Implementation must not start if required specifications are missing, incomplete, or unclear.

### 2. Subagent Strategy
- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

### 3. Self-Improvement Loop
- After ANY correction from the user: update `tasks/lessons.md` with the pattern
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate on these lessons until mistake rate drops
- Review lessons at session start for relevant project

### 4. Verification Before Done
- Never mark a task complete without proving it works
- Ask yourself: "Would a staff engineer approve this?"
- Run tests, check logs, demonstrate correctness
- Each method must be considered complete only after verification by unit testing.
- Before implementing any unit test, enter plan mode and define a Test Plan.
- The Test Plan must be presented to the user and approved before writing test code.
- After approval, implement and execute the test, and demonstrate correctness through results and logs.
- View-layer components are exempt from mandatory unit testing.
- View verification must be performed by running the application on both iOS and Android platforms and confirming correct rendering and behavior.

### 5. Demand Elegance (Balanced)
- For non-trivial changes: pause and ask "is there a more elegant way?"
- If a fix feels hacky: "Knowing everything I know now, implement the elegant solution"
- Skip this for simple, obvious fixes – don't over-engineer

### 6. Autonomous Bug Fixing
- When given a bug report: just fix it. Don't ask for hand-holding
- Point at logs, errors, failing tests – then resolve them
- Zero context switching required from the user

### 7. Failure Handling (Strict)

A "failure" includes any of the following:
- Any failing unit test or integration test
- Build/compile errors, runtime exceptions, or crashes
- Missing, incomplete, or unclear specifications (requirements/design/screen)
- Conflicts with @docs/requirements.md, @docs/design.md, or @docs/domain-design.md
- Phase Gate violations (Exit Criteria not satisfied)
- A divergence between code and @docs (see Document Sync Rule)

When a failure occurs, you MUST follow this procedure:

1. **STOP immediately** — Do not continue implementation. Do not proceed to the next phase.
2. **Identify the root cause** — Explain what failed and why (based on logs/errors/test output). Avoid superficial fixes.
3. **Apply the minimal fix** — Fix only what is necessary. Do not expand scope.
4. **Verify the fix** — Re-run relevant tests. Provide evidence of correctness.
5. **Retry limit (3 attempts)** — If unresolved after 3 attempts: STOP, summarize what was tried, ask the user for clarification.

**Ambiguity rule (Strict)**
- If any required specification is missing, incomplete, or unclear: STOP immediately, ask the user, do not assume behavior.

---

### 8. Scope Control (Strict)

All implementation must remain strictly within the scope of the current task or phase.

- Before implementation: identify the requested change, affected components/layers, and intended scope.
- Do not introduce features, behavior changes, or architectural modifications outside the requested scope.
- Do not implement "nice-to-have" improvements or anticipate future requirements.
- Refactoring is allowed only when strictly necessary to fix a failure, resolve a design conflict, or satisfy defined specifications. Large or structural refactoring requires explicit user approval.
- If expanding scope would significantly improve correctness or design: STOP → explain the rationale → wait for user approval.
- Do not implement work belonging to a future phase. Phase order defined in Development Workflow must be respected.

---

### 9. Definition of Done (Strict)

A task is considered "Done" only when ALL of the following are satisfied:

**Specification Alignment**
- Implementation fully matches @docs/requirements.md, @docs/design.md, and other relevant specifications.
- No assumptions remain undocumented.

**Test Quality**
- Required unit tests are implemented and all pass.
- No failing or skipped tests remain.

**Architectural Integrity**
- Clean Architecture rules are preserved. Dependency direction is not violated.
- Domain rules and invariants remain intact.

**Code Quality**
- No temporary fixes, hacks, or workarounds.
- Changes are minimal and localized.

**Verification Evidence**
- Correctness is demonstrated through tests, logs, or observed behavior.

**No Known Issues**
- No unresolved errors, warnings, or TODOs related to incomplete implementation.

---

### 10. Document Sync Rule

- If a spec change occurs during implementation:
    1. STOP implementation
    2. Update the relevant @docs file first
    3. Obtain user approval on the updated document
    4. Resume implementation
- A divergence between code and @docs is treated as a Failure (Section 7).
- Never let implementation drift ahead of documentation.

---

### 11. Session Start Checklist

At the beginning of every session, before taking any action:

1. Read `tasks/lessons.md` and internalize all relevant rules for this project
2. Check `tasks/todo.md` for incomplete tasks and current status
3. Confirm the current phase and which Exit Criteria have already been satisfied
4. If anything is unclear about the current state, ask the user before proceeding

---

## Lessons Log Format

All entries in `tasks/lessons.md` must follow this format:

```
### [YYYY-MM-DD] <Category: Scope / Architecture / Testing / Docs / Other>
- **What happened**:
- **Root cause**:
- **Rule to prevent recurrence**:
```

---

## Task Management

1. **Plan First**: Write plan to `tasks/todo.md` with checkable items
2. **Verify Plan**: Check in before starting implementation
3. **Track Progress**: Mark items complete as you go
4. **Explain Changes**: High-level summary at each step
5. **Document Results**: Add review section to `tasks/todo.md`
6. **Capture Lessons**: Update `tasks/lessons.md` after corrections

---

## Core Principles

- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs.

---

## Development Workflow

Implementation in this project must follow the phases below in order.
Skipping phases or changing the order of implementation is not allowed.

Each phase defines:
- **References**: documents/resources that MUST be consulted
- **Output**: the artifact that MUST be produced/updated
- **Exit Criteria**: conditions that MUST be satisfied before moving to the next phase

The full prompt for each phase is defined in `@prompts/phase-N.md`.

---

### Phase Gates (Global Exit Rules)

A phase is considered complete only when its required outputs are produced AND the following are satisfied:

- All relevant unit tests must pass (when applicable).
- All required documents for the phase must be created/updated and consistent.
- Any ambiguity or conflicting interpretation must be resolved before proceeding.
- Changes must remain within the intended scope of the current phase.

**User Approval Gates**
- Phase 1 (Requirements) requires user approval.
- Phase 2 (Detailed Design) requires user approval.
- Phase 6 (Screen Design) requires user approval.
- Phase 11 (Store Submission Readiness) requires user approval.
- Phase 12 (Post-Release Maintenance) requires user approval for each change cycle.

---

### Phase 1: Requirements
**References**: N/A (source of truth to be created)
**Output**: `@docs/requirements.md`
**Prompt**: `@prompts/phase-1.md`
**Exit Criteria**:
- `@docs/requirements.md` is complete, consistent, and user-approved.
- No unclear or missing requirements remain.
- Legal document policy is explicitly defined (consent screen, Privacy Policy, Terms of Service, OSS license screen).

---

### Phase 2: Detailed Design
**References**: `@docs/requirements.md`
**Output**: `@docs/design.md`, `@docs/domain-design.md`, `@docs/db-schema.md`
**Prompt**: `@prompts/phase-2.md`
**Exit Criteria**:
- `@docs/design.md` defines: frameworks/libraries/versions, layer structure, dependency direction.
- `@docs/domain-design.md` defines: domain models/invariants, ubiquitous language glossary.
- `@docs/db-schema.md` defines: all tables/fields/types/constraints, relationships, migration strategy.
- All three documents are consistent with `@docs/requirements.md` and user-approved.

---

### Phase 3: Domain Layer
**References**: `@docs/requirements.md`, `@docs/design.md`, `@docs/domain-design.md`
**Output**: Domain layer implementation
**Prompt**: `@prompts/phase-3.md`
**Exit Criteria**:
- All Domain methods are implemented.
- All unit tests pass.
- Domain behavior matches `@docs/domain-design.md`.

---

### Phase 4: UseCase Layer
**References**: `@docs/requirements.md`, `@docs/design.md`, `@docs/domain-design.md`
**Output**: UseCase layer implementation, repository interfaces in Domain
**Prompt**: `@prompts/phase-4.md`
**Exit Criteria**:
- All UseCase methods are implemented and tested.
- Repository interfaces are defined and consistent with domain design.
- No regression in Phase 3 tests.

---

### Phase 5: Repository (Data) Layer
**References**: `@docs/requirements.md`, `@docs/design.md`, `@docs/domain-design.md`, `@docs/db-schema.md`
**Output**: Repository implementations in Data layer
**Prompt**: `@prompts/phase-5.md`
**Exit Criteria**:
- Repository implementations conform to Domain interfaces and `@docs/db-schema.md`.
- All unit tests pass (in-memory).
- Data mapping preserves domain constraints.
- No regression in Phase 3 and 4 tests.

---

### Phase 6: Screen Design
**References**: `@docs/requirements.md`, `@docs/design.md`, Domain/UseCase/Data implementations
**Output**: `@docs/screen.md`, `@docs/images-design.md`
**Prompt**: `@prompts/phase-6.md`
**Exit Criteria**:
- `@docs/screen.md` is complete: all states, events, transitions, ViewModel states, UseCase mappings defined.
- `@docs/images-design.md` is complete: all assets with generation method defined.
- Required legal screens are included. Navigation to legal docs is within 2 taps.
- User approval obtained for both documents.

---

### Phase 7: UI Design (Figma Make)
**References**: `@docs/screen.md`, `@docs/images-design.md`, Figma design (via Figma MCP)
**Output**: Figma UI design validated against `@docs/screen.md` and `@docs/images-design.md`
**Prompt**: `@prompts/phase-7.md`

**Role Separation (Strict)**
- The primary actor is **Figma Make (ClaudeCode Sonnet 4.6)**, not ClaudeCode.
- ClaudeCode must NOT design UI layout, styling, visual hierarchy, spacing, colors, or component arrangement.
- ClaudeCode accesses the Figma design via **Figma MCP** only. No URL or file sharing required.
- ClaudeCode must remain idle regarding UI creation until the user confirms the Figma design is complete.

**Exit Criteria**:
- Figma design accessed via Figma MCP and confirmed retrievable.
- All screen states, conditional UI variations, and interaction flows are represented.
- All assets from `@docs/images-design.md` are present in the Figma design.
- User explicitly confirms Phase 7 completion.

---

### Phase 8: ViewModel Implementation
**References**: `@docs/screen.md`, `@docs/requirements.md`, UseCase interfaces
**Output**: ViewModel implementation
**Prompt**: `@prompts/phase-8.md`
**Exit Criteria**:
- All state transitions behave as defined in `@docs/screen.md`.
- Tests for state transitions pass.
- No regression in Phase 3, 4, 5 tests.

---

### Phase 9: View Implementation (Screen-based Vertical Slice)
**References**: `@docs/screen.md`, `@docs/requirements.md`, `@docs/images-design.md`, ViewModel (Phase 8), Figma design (via Figma MCP)
**Output**: View implementation per screen, View-ViewModel binding
**Prompt**: `@prompts/phase-9.md`
**Exit Criteria (Per Screen)**:
- Screen runs successfully on both iOS and Android.
- View correctly reflects all states defined in `@docs/screen.md`.
- All image and icon assets are resolved.
- No critical rendering or interaction defects.
- Screen completion is explicitly confirmed before moving to the next screen.

---

### Phase 10: Integration
**References**: `@docs/requirements.md`, `@docs/screen.md`, View+ViewModel (Phase 9), DI configuration
**Output**: Application-wide integration verification, final DI configuration, E2E validation
**Prompt**: `@prompts/phase-10.md`
**Exit Criteria**:
- All screens function together correctly.
- Cross-screen navigation behaves as expected.
- All critical user flows are manually verified.
- DI configuration is stable and consistent.
- No blocking runtime errors or crashes.
- User confirms application behavior matches expectations.

---

### Phase 11: Store Submission Readiness
**References**: `@docs/requirements.md`, `@docs/store-submission.md`, App Store/Google Play guidelines, implemented application
**Output**: `@docs/store-submission.md`, all required fixes implemented
**Prompt**: `@prompts/phase-11.md`

**Role Separation (Strict)**
- ClaudeCode audits the project and implements fixes within the codebase.
- Store console actions (uploading builds, filling listings, submitting for review) must be performed by the user.
- ClaudeCode must NOT claim submission is complete without explicit user confirmation.

**Exit Criteria**:
- `@docs/store-submission.md` is complete and user-approved.
- All audit items in the Store Submission Checklist are resolved.
- User has created the release build and confirmed it launches successfully on both iOS and Android.
- No known policy violations remain.
- User explicitly confirms Phase 11 completion and submission readiness.
- Upon Phase 11 completion, the project enters Phase 12 for all future changes.

---

### Phase 12: Post-Release Maintenance
**Purpose**: Repeating maintenance cycle after Phase 11. Every bug fix, feature addition, or architectural change is handled here.
**References**: All @docs files (current released state)
**Output**: Updated source code and @docs files reflecting the requested change
**Prompt**: `@prompts/phase-12.md`

**Change Classification (MANDATORY)**

Before any implementation, classify the change and obtain user approval:

| Category | Definition | Phases to Re-execute |
|----------|-----------|----------------------|
| A: Bug Fix | Code-level fix within existing specifications. | Affected layer only (minimal) |
| B: Spec Change (Minor) | Behavior change within existing screens or features. | requirements.md update → affected Phase 3–9 (selective) |
| C: Feature / Screen Addition | New requirement added. New screens or features introduced. | requirements.md → Phase 1-equivalent interview → affected Phase 2–9 |
| D: Architecture Change | Changes to design.md, db-schema.md, or layer structure. | requirements.md → Phase 2–5 (selective) |

**Release Decision Rules**
- Category A: Version number update only. Re-run Phase 11 Category 1–2 checks only.
- Category B / C / D: Re-run Phase 11 in full if new screens, permissions, or privacy-related changes are involved.
- Always update the version/build number for any release.

**Exit Criteria (Per Change Cycle)**
- The requested change is fully implemented and verified.
- All affected @docs files are updated and consistent with the implementation.
- All relevant tests pass (no regression in Phase 3, 4, 5, and 8 tests).
- If a release is required: release build succeeds and store-submission.md is updated.
- User explicitly confirms the change cycle is complete.