# Phase 7: UI Design Evaluation

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Phase Gates).

## Language Rules (MANDATORY)
- Questions, explanations, and inconsistency reports to the user: English

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 6 is complete and user-approved
- [ ] `@docs/screen.md` is complete and approved
- [ ] `@docs/images-design.md` is complete and approved

## Phase 7 Responsibilities (MANDATORY)

This phase does NOT design UI.
**Figma Make (ClaudeCode Sonnet 4.6)** is responsible for generating UI and image assets.
ClaudeCode's sole responsibility is to verify consistency between:
- Figma design (accessed via Figma MCP)
- `@docs/screen.md`
- `@docs/images-design.md`

**ClaudeCode must NOT:**
- Design UI layout, styling, visual hierarchy, spacing, colors, or component arrangement
- Reinterpret or modify screen behavior defined in `@docs/screen.md`
- Proceed to Phase 8 without explicit user confirmation

## Figma MCP Access Rules (MANDATORY)
- Figma design must only be accessed via Figma MCP. No URL or file sharing required.
- Once the user confirms the Figma design is complete, immediately access it via Figma MCP.
- If access fails: STOP → ask the user to confirm the Figma MCP connection.

## Evaluation Rules (STRICT)
- Evaluate ONE screen at a time in order (batch evaluation is prohibited)
- After each screen evaluation: STOP → obtain user confirmation before moving to the next screen
- If an inconsistency is found: STOP → report using the Inconsistency Report Format below
- Do not proceed to the next screen until the inconsistency is resolved

## Evaluation Checklist (per screen)

Verify ALL of the following for each screen against `@docs/screen.md` and `@docs/images-design.md`:

**Behavior (from screen.md)**
- [ ] All screen states are visually represented
- [ ] All conditional rendering rules are reflected
- [ ] All user events are visually supported
- [ ] All state transitions are represented as interactions
- [ ] Error states and loading states are represented
- [ ] Navigation transitions are represented
- [ ] No UI elements contradict the behavior defined in screen.md

**Image and Icon Assets (from images-design.md)**
- [ ] UI image assets used in the screen are present in the Figma design
- [ ] Header icons and navigation icons used in the screen are present
- [ ] The appearance of each asset matches the definition in images-design.md (style, color concept, etc.)
- [ ] "User-provided" assets are appropriately represented as placeholders

**App Icon (verified once after all screens are evaluated)**
- [ ] App icon design matches the concept, colors, and style in images-design.md

## Inconsistency Report Format
If an inconsistency is found: STOP immediately and report in the following format:

```
- **Target Screen**:
- **Type of Inconsistency**: Missing state / Wrong behavior / Undefined interaction / Missing asset / Wrong asset style / Other
- **Definition in screen.md or images-design.md**:
- **Current state in Figma design**:
- **Recommended Action**: Fix Figma / Review screen.md / Review images-design.md / Ask user for decision
```

Do not proceed until the user resolves the reported inconsistency.

## SubAgent Usage ①: After all screens are evaluated
→ Launch `ui-designer` to verify:
  - Whether all interaction patterns defined in screen.md are reflected in the Figma design
  - Whether there are any overlooked issues in cross-screen transition consistency (navigation patterns, back navigation, etc.)
  - Whether error states, empty states, and loading states are represented consistently across all screens
→ Present any detected issues to the user and resolve them by mutual agreement before moving to the completion summary.

## Completion Summary Format
After all screens are evaluated, present a summary including:

- List of evaluated screens
- Inconsistencies found and their resolution status (behavior and assets both)
- Any unresolved issues (if applicable)
- Whether Phase 7 can be marked as complete

## Phase 8 Transition Block (MANDATORY)
Proceeding to Phase 8 is absolutely prohibited until ALL of the following are satisfied:

1. Figma design has been accessed via Figma MCP
2. Evaluation checklist is complete for all screens (behavior and assets both)
3. App icon has been verified against `@docs/images-design.md`
4. `ui-designer` interaction consistency check completed
5. Completion summary has been presented to the user
6. User has explicitly confirmed Phase 7 completion

If the user attempts to proceed to Phase 8 without satisfying all of the above:
STOP → request the missing confirmation.

## Exit Criteria (Phase 7)

Mark Phase 7 as complete only when ALL of the following are satisfied:

- [ ] Figma design has been accessed via Figma MCP
- [ ] All screens have been individually reviewed
- [ ] All evaluation checklists are complete (behavior and assets both)
- [ ] All states and interactions match `@docs/screen.md`
- [ ] All assets defined in `@docs/images-design.md` are present in the Figma design
- [ ] App icon matches `@docs/images-design.md`
- [ ] All inconsistencies have been resolved
- [ ] `ui-designer` interaction consistency check completed
- [ ] Completion summary has been presented
- [ ] User has explicitly confirmed Phase 7 completion

## Start
First ask the user:

"Has the UI design by Figma Make (ClaudeCode Sonnet 4.6) already been completed?"

**If complete:**
Access the design via Figma MCP, review the screen list in `@docs/screen.md`
and the asset list in `@docs/images-design.md`, and propose an evaluation order.
No URL or file sharing is required. Obtain user approval before starting evaluation.

**If not complete:**
Wait until the user confirms completion of the design by Figma Make (ClaudeCode Sonnet 4.6).
Do not proceed to Phase 8 under any circumstances.