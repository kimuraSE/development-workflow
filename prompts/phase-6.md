# Phase 6: Screen Design

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Content of `@docs/screen.md` and `@docs/images-design.md`: English

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 5 is complete (all Repositories implemented, all tests pass)
- [ ] `@docs/requirements.md` is approved
- [ ] `@docs/design.md` is approved

## Role
Operate as UX Architect, Product Interaction Designer, and Clean Architecture Specialist.

## Responsibility Definition for screen.md (MANDATORY)

**✔ What screen.md MUST define:**
Screen states, conditional rendering rules, user events, state transitions,
ViewModel state definitions, UseCase mappings, UX rules, error/loading states, and navigation.

**✘ What screen.md MUST NOT define:**
UI layout, positioning, style, colors, spacing, component arrangement, or pixel-level details.

If visual or layout design is attempted: STOP → correct immediately.

## Execution Order (mandatory)

**Stage 1** → Complete `@docs/screen.md`
**Stage 2** → Complete `@docs/images-design.md` (may only start after Stage 1 is approved)

---

## Stage 1: Screen Definition

### Screen List (must be done before starting)
Review `@docs/requirements.md` Section 4 to identify required legal screens,
then list all screens. Include the following legal screens if "required" (STOP if any are missing):

- First-launch user consent screen (if Section 4.1 is "required")
- Privacy Policy screen (if Section 4.2 is "in-app page")
- Terms of Service screen (if Section 4.3 is "in-app page")
- OSS license list screen (if Section 4.4 is "required")
- Legal link section within the Settings screen

Present the screen list and definition order to the user and obtain approval before starting per-screen definition.

### Per-Screen Definition Order (mandatory — no skipping)
Define each screen in the following order. Complete all 9 steps before moving to the next screen.

1. Screen purpose and user goal
2. Initial state of the screen
3. List of user events
4. State transitions
5. Conditional rendering rules
6. ViewModel state definitions
7. UseCase mappings
8. Error states and loading states
9. Navigation (destination screens)

### Additional Rules for Legal Screens

**First-launch user consent screen (if required)** → explicitly define ALL of the following:
- Two states: not-yet-agreed and already-agreed (condition for skipping on relaunch if already agreed)
- Condition that blocks app entry until the user taps "Agree and Start"
- UseCase mapping for persisting the agreement state
- Transition: tap agree button → persist agreement state → navigate to home screen
- Links to Privacy Policy and Terms of Service (tap → respective screen or URL)

**Legal link section in the Settings screen** → explicitly define ALL of the following:
- Existence of a dedicated section for legal documents
- Navigation events: navigate to Privacy Policy, Terms of Service, and OSS license list
- Each legal link is reachable within 2 taps from the main screen
- Destination for each link (in-app screen or external URL)

### SubAgent Usage ①: After Stage 1 is complete (after all screens are defined)
→ Launch `ux-researcher` to verify:
  - Whether state transitions across all screens have any gaps or contradictions
  - Whether error states and loading states are appropriately defined
  - Whether there are unnatural transitions or dead-ends from a user flow perspective
  - Whether UI layout or visual elements have crept into the definitions
→ Present any detected issues to the user and resolve them by mutual agreement before requesting approval.

---

## Stage 2: Image Asset Definition

May only start after `@docs/screen.md` is approved. Ask ONE question at a time in the order below.

1. **UI image assets**: presence of images, illustrations, banners, or placeholders used in each screen
   - If present: name, screen used in, size/ratio, generation method (Figma Make / user-provided)
2. **Header icons and navigation icons**: presence of icons used in headers or navigation
   - If present: icon name, location used, style concept (simple, outlined, filled, etc.)
3. **App icon**: design concept, color concept, style, generation method

After all questions are answered, summarize the asset list and confirm with the user before creating `@docs/images-design.md`.

### SubAgent Usage ②: After Stage 2 is complete
→ Launch `feedback-synthesizer` to verify:
  - Whether all assets referenced in `@docs/screen.md` are defined in `@docs/images-design.md`
  - Whether any asset requirements mentioned in `@docs/requirements.md` are missing
  - Whether the generation method (Figma Make / user-provided) is specified for each asset
→ If issues are found, present them and resolve them before requesting approval.

### images-design.md Template (structure must not be changed)
```
# Image and Icon Asset Design Document

## 1. UI Image Assets
| Asset Name | Screen Used In | Size / Ratio | Generation Method | Notes |

## 2. Header Icons and Navigation Icons
| Asset Name | Location Used | Style | Generation Method | Notes |

## 3. App Icon
### 3.1 Design Concept
### 3.2 Color Concept
### 3.3 Style
### 3.4 Generation Method (Figma Make / User-Provided)

## 4. Approval
```

## Document Update Rules
- After each decision, update and present only the relevant section (full re-display is not required)
- If a contradiction with existing content is detected: STOP → explain → resolve → then update
- Always verify consistency with `@docs/requirements.md`, `@docs/design.md`, and UseCase behavior

## Completion Criteria (confirm all items before requesting approval)

**screen.md**
- [ ] All screens are listed
- [ ] All 9 steps are completed for each screen
- [ ] No UI layout or visual elements are included
- [ ] All legal screens required by requirements.md Section 4 are included
- [ ] First-launch consent screen not-yet-agreed and already-agreed states and transitions are defined (if required)
- [ ] Legal document links are defined in the Settings screen
- [ ] Each legal link is reachable within 2 taps from the main screen
- [ ] Consistency with `@docs/requirements.md` is verified
- [ ] `ux-researcher` UX quality check completed

**images-design.md**
- [ ] UI image asset list is defined (explicitly state "none" if not applicable)
- [ ] Header icon and navigation icon list is defined (explicitly state "none" if not applicable)
- [ ] App icon concept, colors, and style are defined
- [ ] Generation method is specified for each asset
- [ ] `feedback-synthesizer` asset coverage check completed

## Start
First review `@docs/requirements.md` Section 4 to identify required legal screens,
then list all screens (including legal screens) based on `@docs/requirements.md` and `@docs/design.md`,
and propose a definition order based on the user flow.
Obtain user approval before starting per-screen definition.
