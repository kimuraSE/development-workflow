# Phase 12: Post-Release Maintenance

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

This phase is a repeating maintenance cycle after the first store release.
Use this prompt for every change cycle.

## Language Rules (MANDATORY)
- Questions, explanations, and document updates: English
- Code, variable names, and comments: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 11 is complete (first store release delivered)
- [ ] All `@docs` files exist and reflect the current released state:
  - `@docs/requirements.md` / `@docs/design.md` / `@docs/domain-design.md`
  - `@docs/db-schema.md` / `@docs/screen.md` / `@docs/images-design.md`
- If any `@docs` file is missing or out of sync with the codebase: STOP → ask the user

Do not begin implementation until the change request is classified and user-approved.

## Change Cycle Execution Order (mandatory — no skipping)

---

### Step 1: Change Request Intake
Receive the change request from the user and confirm understanding.
If the request is ambiguous, ask ONE clarifying question at a time.
Do not assume intent. Do not begin classification until the request is fully understood.

---

### Step 2: Change Classification

### SubAgent Usage ①: Impact analysis before classification
→ Launch `feedback-synthesizer` to analyze:
  - Which `@docs` files and phases may be affected by the change request
  - Similar past change patterns (refer to content in `tasks/lessons.md`)
  - Decision criteria for cases where the category boundary is ambiguous
→ Based on the analysis results, classify using the table below and present the classification and reasoning to the user.

| Category | Definition | Phases to Re-execute |
|---|---|---|
| **A: Bug Fix** | Code-level fix within existing specifications. No document changes required. | Affected layer only (minimal) |
| **B: Spec Change (Minor)** | Behavior change within existing screens or features. Requires document updates. | Update requirements.md → affected Phase 3–9 (selective) |
| **C: Feature / Screen Addition** | New requirement added. New screens or features introduced. | Update requirements.md → Phase 1-equivalent interview → affected Phase 2–9 |
| **D: Architecture Change** | Changes to design.md, db-schema.md, or layer structure. | Update requirements.md → affected Phase 2–5 |

If the change spans multiple categories → classify as the highest category (D > C > B > A)
If classification is unclear → STOP → ask the user

Obtain explicit user approval before proceeding to Step 3.

---

### Step 3: Impact Assessment
Based on the classification, identify ALL of the following:
- `@docs` files that need to be updated
- Phases that need to be re-executed
- Tests that need to be re-run
- Whether a new store release is required (see Release Decision Rules)

Present the impact assessment to the user and obtain approval.

---

### Step 4: Document Update (for Categories B/C/D)

**For Categories B/C/D:**
1. Update all affected `@docs` files BEFORE any implementation
2. Present each updated document to the user
3. Obtain user approval for each document
4. Do not begin implementation until all `@docs` updates are approved

**For Category A:**
- Document updates are not required before implementation
- If a spec gap is discovered during implementation → STOP → update the relevant `@docs` file → obtain approval → resume implementation

---

### Step 5: Implementation

Re-execute only the identified phases, following all existing phase rules strictly.

**Category A (Bug Fix):**
- Apply the minimal fix to the affected layer only
- No refactoring of unrelated code, no scope expansion
- Re-run all Phase 3, 4, 5, and 8 tests to confirm no regression

**Category B (Spec Change — Minor):**
- Follow the rules of each re-executed phase as defined in `@CLAUDE.md`
- Start from the earliest affected phase and proceed in order (no phase skipping)

**Category C (Feature / Screen Addition):**
- Conduct a Phase 1-equivalent requirements interview for the new feature/screen only
- Add new requirements to `@docs/requirements.md` (modifying existing requirements is prohibited in principle)
- Execute affected phases in order (Phase 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 as needed)
- For new screens: update `@docs/screen.md` and `@docs/images-design.md` in Phase 6
- For new screens: verify new Figma frames via Figma MCP in Phase 7

**Category D (Architecture Change):**
- Update `@docs/design.md` and/or `@docs/db-schema.md` first
- Obtain user approval before any code changes
- Execute affected phases in order (Phase 2 → 3 → 4 → 5 as needed)
- Re-run all Phase 3, 4, and 5 tests after completion

---

### Step 6: Verification

### SubAgent Usage ②: Regression analysis after implementation is complete
→ Launch `test-results-analyzer` to analyze:
  - Whether there are any regression patterns across all Phase 3, 4, 5, and 8 test results
  - Whether any tests were affected by the current change
  - Whether test failures are concentrated in a specific layer or feature
→ Present the analysis results to the user before proceeding to Step 7.

After implementation is complete:
- Re-run all Phase 3, 4, 5, and 8 tests to confirm no regression
- Present test results as evidence
- If View layer changes are present: verify behavior on both iOS and Android platforms

---

### Step 7: Release Decision

### SubAgent Usage ③: During release decision
→ Launch `workflow-optimizer` to analyze:
  - The minimum required re-execution scope for Phase 11 based on the change content
  - Decision criteria for whether a new store release is required
→ Based on the analysis results, make a determination using the table below and present it to the user.

| Category | Release Action |
|---|---|
| A: Bug Fix | Update version number only. Re-verify Phase 11 Category 1–2 only. |
| B: Spec Change (Minor) | Re-execute relevant Phase 11 categories based on the change content. |
| C: Feature / Screen Addition | Re-execute Phase 11 in full if new screens, permissions, or privacy-related changes are involved. Otherwise re-execute relevant categories only. |
| D: Architecture Change | Re-verify Phase 11 Category 1, 5, and 6. |

- Always update the version / build number for any release
- Present the release decision to the user and obtain approval before proceeding
- If a new release is required: update `@docs/store-submission.md`

---

## Scope Control (STRICT)
- Fix or add only what was requested
- No refactoring of unrelated code
- No addition of unrequested features or improvements
- No `@docs` changes beyond what the current change requires
- If scope drift is detected: STOP → ask the user

## Exit Criteria (per change cycle)

Mark the change cycle as complete only when ALL of the following are satisfied:

- [ ] The requested change is fully implemented and verified
- [ ] All affected `@docs` files are updated and consistent with the implementation
- [ ] Phase 3, 4, 5, and 8 tests all pass (no regression)
- [ ] View layer changes are verified on both iOS and Android platforms (if applicable)
- [ ] `test-results-analyzer` regression analysis completed
- [ ] `workflow-optimizer` release scope optimization analysis completed
- [ ] If a release is required: release build succeeds and `@docs/store-submission.md` is updated
- [ ] User has explicitly confirmed the change cycle is complete

## Start
Please describe what change, fix, or addition you would like to make.
After reviewing the request, the change will be classified (A–D) and
the affected phases will be organized in plan mode and proposed.
Implementation will begin after user approval.