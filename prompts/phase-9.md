# Phase 9: View Implementation (Screen-based Vertical Slice)

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Code conventions: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 8 is complete and all tests pass
- [ ] All target ViewModels are implemented and verified
- [ ] Phase 7 Figma design is accessible via Figma MCP and approved
- [ ] `@docs/screen.md` is complete and finalized
- [ ] `@docs/images-design.md` is complete and approved

## Objective
Using `@docs/screen.md`, `@docs/images-design.md`, Figma MCP, and the implemented ViewModels,
implement the View layer one screen at a time and connect it to the ViewModel (vertical slice approach).

## One-Screen-at-a-Time Rule (MANDATORY)

Execute the following steps for each screen in order. Simultaneous implementation of multiple screens is prohibited.

1. Confirm the target screen with the user
2. Access the corresponding Figma frame via Figma MCP
3. Identify all assets in the Figma frame and resolve them (see Asset Resolution Rules below)
4. Implement only the target screen
5. Verify on both iOS and Android platforms

### SubAgent Usage ①: After iOS/Android verification for each screen
→ Launch `mobile-app-builder` to verify:
  - Whether there are any behavioral differences between iOS and Android (platform-specific rendering issues)
  - Whether all states in `@docs/screen.md` work correctly on both platforms
  - Whether ViewModel binding works correctly on both platforms
→ If issues are detected, present them to the user and resolve them before proceeding to the Definition of Done checklist.

6. Present the Definition of Done checklist to the user
7. Obtain explicit user approval
8. Move to the next screen

## Figma MCP Access Rules (MANDATORY)
- Figma design must only be accessed via Figma MCP. No URL or file sharing required.
- Before implementing each screen, access the corresponding Figma frame to retrieve the layout and styling.
- If access fails: STOP → ask the user to confirm the Figma MCP connection.

## Asset Resolution Rules (MANDATORY)
Before implementing each screen, identify all assets in the Figma frame and resolve them
by referring to `@docs/images-design.md` as follows.
Do not start View implementation until all assets are resolved.

| Generation Method | Action |
|---|---|
| Figma Make generated | Retrieve the asset from Figma MCP and place it in the appropriate project directory |
| User-provided | Ask the user: "Please place [asset name] at [path]. Let me know when done." Wait for the user's confirmation before proceeding |
| Reproduced in code | Implement the asset in code (no external files) |
| Not defined in images-design.md | STOP → ask the user: "[Asset name] is not defined in images-design.md. How should this be handled?" |

## ViewModel Binding Rules
- The View observes ViewModel state and reflects the states defined in `@docs/screen.md`
- User interactions trigger ViewModel events (only those defined in `@docs/screen.md`)
- Adding state or event handling logic directly in the View layer is prohibited
- Calling UseCases directly from the View is prohibited
- If behavior not defined in `@docs/screen.md` is required: STOP → ask the user

## Scope (per screen — mandatory)

**Allowed:**
- View implementation for the target screen
- UI rendering based on ViewModel state
- User event handling that triggers ViewModel events
- Minimal dependency wiring required to run the screen
- Asset placement (Figma Make generated) or code implementation (reproduced in code)

**Prohibited:**
- Modifying other screens
- Modifying ViewModel behavior
- Adding state or logic directly to the View layer
- Proceeding with unresolved assets
- Creating files outside the View layer boundary defined in `@docs/design.md`
- Modifying `@docs/screen.md` without following the change rules below

## If screen.md Changes Are Required
1. Immediately STOP View implementation → explain the gap or inconsistency to the user
2. Update `@docs/screen.md` → obtain user approval
3. Resume View implementation from the affected point

## Verification Requirements (MANDATORY)
Confirm ALL of the following for the target screen:

- [ ] View reflects all states defined in `@docs/screen.md`
- [ ] UI correctly reacts to ViewModel state changes
- [ ] User interactions trigger the correct ViewModel events
- [ ] State transitions work correctly
- [ ] Rendering matches the Figma design retrieved via Figma MCP
- [ ] All image and icon assets are correctly displayed (no missing or broken assets)
- [ ] No runtime errors
- [ ] Works correctly on both iOS and Android platforms

## Regression Rule
After each screen implementation is complete:
- Re-run all Phase 3, 4, 5, and 8 tests
- Confirm no regression has occurred
- Present test results as evidence before marking as "Done"

## Definition of Done (per screen)

Mark as "Done" only when ALL of the following are satisfied:

- [ ] View reflects all states defined in `@docs/screen.md`
- [ ] UI matches the Figma design retrieved via Figma MCP
- [ ] All user interactions trigger the correct ViewModel events
- [ ] State transitions work correctly
- [ ] All assets are resolved and correctly displayed
- [ ] Works correctly on both iOS and Android platforms
- [ ] No runtime errors or critical rendering defects
- [ ] Phase 3, 4, 5, and 8 tests all pass (no regression)
- [ ] `mobile-app-builder` cross-platform quality check completed
- [ ] User has explicitly approved the screen

## Exit Criteria (Phase 9)

### SubAgent Usage ②: During Exit Criteria verification (after all screens are complete)
→ Launch `test-results-analyzer` to analyze:
  - Whether there are any regression patterns across all Phase 3–8 test results
  - Whether there are any common issues latent across multiple screens
  - Whether Definition of Done conditions are consistently met across all screens as recorded
→ Present the analysis results to the user before proceeding to final confirmation.

Mark Phase 9 as complete only when ALL of the following are satisfied:

- [ ] All screens are implemented and verified
- [ ] Definition of Done conditions are met for all screens
- [ ] All assets across all screens are resolved
- [ ] All screens work correctly on both iOS and Android platforms
- [ ] Phase 3, 4, 5, and 8 tests all pass (no regression)
- [ ] `mobile-app-builder` cross-platform check completed for all screens
- [ ] `test-results-analyzer` cross-phase test analysis completed
- [ ] User has confirmed completion of all screens

## Start
Review the content of `@docs/screen.md`, `@docs/images-design.md`, and `@docs/design.md`,
list all screens that need to be implemented,
plan the implementation order based on the user flow in plan mode, and propose it to the user.
Before implementing each screen, access the corresponding design via Figma MCP
and resolve the assets used by cross-referencing with `@docs/images-design.md`.
No URL or file sharing is required.
Obtain user approval before starting implementation of the first screen.