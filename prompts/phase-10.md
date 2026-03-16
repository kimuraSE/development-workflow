# Phase 10: Integration

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete: STOP → guide the user back to the relevant phase.

- [ ] Phase 9 is complete for all screens (View + ViewModel binding implemented)
- [ ] Phase 3, 4, 5, and 8 tests all pass
- [ ] Build succeeds on both iOS and Android platforms

## Goal (Phase 10 Deliverables)
- Application-wide integration verification
- Cross-screen navigation and behavior verification
- App-wide DI configuration verification
- End-to-end user flow verification
- Final confirmation of app behavior with the user

This phase focuses solely on system-wide verification and stabilization.

## Scope (Phase 10 only)

**Allowed:**
- Cross-screen navigation verification
- E2E user flow verification
- App-wide state consistency verification
- DI configuration verification
- Detection and fixing of integration issues (minimal fixes only)
- Detection and fixing of runtime errors and crashes (minimal fixes only)

**Prohibited:**
- Adding new features
- Changing screen specs or UI
- Changing behavior definitions in `@docs/screen.md`
- Large-scale architectural refactoring
- Modifying ViewModel or Domain logic beyond minimal integration fixes

If spec changes are required → STOP → return to Phase 6
If Domain/UseCase changes are required → STOP → return to the relevant phase

## Execution Order (mandatory — no skipping)

Before starting, review `@docs/requirements.md` and `@docs/screen.md`,
list all critical user flows that need to be verified,
plan the verification order in plan mode, present it, and obtain approval before starting.

1. Verify app startup and initialization
2. Verify DI configuration
3. Verify cross-screen navigation
4. Verify critical user flows end-to-end
5. Verify app-wide state consistency
6. Final Regression (all Phase 3, 4, 5, and 8 tests)
7. Report findings and confirm behavior with the user

## Integration Verification Checklist

### 1. App Startup
- [ ] App starts without errors on both iOS and Android
- [ ] Initial screen displays correctly

### 2. DI Configuration
- [ ] All DI modules are correctly configured
- [ ] All dependencies are correctly resolved
- [ ] No circular dependencies exist

### 3. Navigation
- [ ] All screen transitions match the navigation definitions in `@docs/screen.md`
- [ ] Back navigation works correctly
- [ ] Deep links (if defined) work correctly

### 4. E2E User Flows

### SubAgent Usage ①: During E2E user flow verification (step 4)
→ Launch `mobile-app-builder` to verify:
  - Whether all critical flows in `@docs/requirements.md` can run to completion on both iOS and Android
  - Whether error cases mid-flow are correctly handled on both platforms
  - Whether app state is correctly preserved after screen transitions on both platforms
→ Report any detected issues using the Issue Report Format and resolve them before moving to the next step.

- [ ] All critical flows in `@docs/requirements.md` work
- [ ] Each flow can run from start to finish
- [ ] Error cases mid-flow are correctly handled

### 5. State Consistency
- [ ] App state is correctly preserved after screen transitions
- [ ] Recovery from error states works correctly
- [ ] Loading states work correctly across all screens

### 6. Final Regression

### SubAgent Usage ②: During Final Regression (step 6)
→ Launch `test-results-analyzer` to analyze:
  - Whether there are any regression patterns across all Phase 3, 4, 5, and 8 test results
  - Whether fixes made during the integration phase have affected existing tests
  - Whether test failures are concentrated in a specific layer or feature
→ Present the analysis results to the user before proceeding to Exit Criteria verification.

- [ ] Phase 3 (Domain) all tests pass
- [ ] Phase 4 (UseCase) all tests pass
- [ ] Phase 5 (Repository) all tests pass
- [ ] Phase 8 (ViewModel) all tests pass

Confirm all checklist items before requesting user approval.

## Issue Report Format
If an integration issue is found: STOP immediately and report in the following format:

```
- **Location**: Screen name / Flow name
- **Issue Type**: Navigation / DI / State / Crash / Behavior mismatch / Other
- **Expected Behavior**: (as defined in @docs/requirements.md or @docs/screen.md)
- **Actual Behavior**:
- **Recommended Action**: Minimal fix proposal
```

Do not proceed to the next step until the user resolves or approves the reported issue.

## Exit Criteria (Phase 10)

Mark Phase 10 as complete only when ALL of the following are satisfied:

- [ ] All integration verification checklist items are confirmed
- [ ] All screens work together correctly
- [ ] Cross-screen navigation works correctly
- [ ] All critical E2E user flows are verified
- [ ] DI configuration is stable and consistent
- [ ] No blocking runtime errors or crashes
- [ ] `mobile-app-builder` E2E cross-platform verification completed
- [ ] `test-results-analyzer` Final Regression analysis completed
- [ ] User has confirmed and approved the app behavior

## Start
Review the content of `@docs/requirements.md` and `@docs/screen.md`,
list all critical user flows that need to be verified,
plan the verification order in plan mode, and propose it to the user.
Obtain user approval before starting integration verification.