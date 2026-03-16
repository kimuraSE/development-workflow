# Phase 11: Store Submission Readiness

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Content of `@docs/store-submission.md`: English
- Code changes: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete: STOP → guide the user back to the relevant phase.

- [ ] Phase 10 is complete and user-approved
- [ ] Debug build succeeds on both iOS and Android platforms
- [ ] Phase 3, 4, 5, and 8 tests all pass

## Role Separation (MANDATORY)

**ClaudeCode responsibilities:**
- Audit the project codebase and identify all gaps and issues
- Implement all required fixes within the codebase
- Create and maintain `@docs/store-submission.md`
- Instruct the user to create the release build after all fixes are complete

**User responsibilities (ClaudeCode must NOT perform these):**
- Creating the release build
- Uploading builds to App Store Connect or Google Play Console
- Entering store listing metadata
- Submitting for review / responding to reviewer feedback

Do not declare submission complete without explicit user confirmation.
For items that only the user can perform, provide clear instructions.

## Audit Scope (6 categories — both iOS and Android platforms)

### Category 1: Build and Release Configuration
- [ ] Release build configuration is correctly set up (debug → release)
- [ ] Signing configuration is correctly set up (iOS: Provisioning Profile / Android: Keystore)
- [ ] App version and build number are correctly set
- [ ] Production API endpoints and environment settings are in use
- [ ] Debug flags and log output are disabled in the release build

### Category 2: App Metadata
- [ ] App name is consistent and finalized across both platforms
- [ ] Bundle ID (iOS) / Application ID (Android) is correctly configured
- [ ] App icons are prepared based on the concept in `@docs/images-design.md` Section 3, in all required sizes for both platforms
- [ ] Splash screen is configured
- [ ] Version string is correctly displayed to users

### Category 3: Permissions, Privacy, and Legal Accessibility

**3.1 Permissions**
- [ ] Only required permissions are declared (no over-requesting)
- [ ] iOS: All permission usage descriptions are defined in Info.plist
- [ ] Android: Permission declarations in AndroidManifest.xml are minimal

**3.2 Privacy Policy**
- [ ] Privacy Policy is publicly accessible at a URL
- [ ] Privacy Policy is accessible from within the app (Settings screen or onboarding)
- [ ] Reachable within 2 taps from the main screen
- [ ] Data collected, purpose of use, and third-party sharing are clearly stated
- [ ] Contact information and last updated date are included
- [ ] iOS: App Privacy Nutrition Label declarations match the actual implementation
- [ ] Android: Data safety section declarations match the actual implementation

**3.3 Terms of Service**
- [ ] Terms of Service document exists
- [ ] Terms of Service is accessible from within the app (Settings screen or onboarding)
- [ ] Reachable within 2 taps from the main screen
- [ ] Scope of use, prohibited activities, disclaimer, and governing law are included

**3.4 In-App Legal Link Verification**
- [ ] A dedicated section for legal documents exists in the Settings screen (or equivalent)
- [ ] Both Privacy Policy link and Terms of Service link are displayed in this section
- [ ] Both links open correctly and display the intended content
- [ ] Legal links are not hidden behind non-obvious navigation

### Category 4: Store Policy and Guidelines Compliance

**4.1 Store Guidelines**
- [ ] No prohibited content (violence, adult content, hate speech, discrimination, etc.)
- [ ] In-app purchase / subscription implementation complies with store guidelines (if applicable)
- [ ] User data handling complies with platform policies
- [ ] App Store: Complies with Human Interface Guidelines (HIG) core principles
- [ ] App Store: Complies with Guideline 4.0 (Design)
- [ ] App Store: Complies with Guideline 5.1 (Privacy)
- [ ] Google Play: Target API Level meets the latest requirements
- [ ] Google Play: Complies with Families Policy (for children's apps, if applicable)
- [ ] App description and screenshots accurately reflect actual functionality

**4.2 Copyright and Asset Licenses**
- [ ] Rights are owned or licensed for all images, illustrations, and icons used in the app (cross-check with the full asset list in `@docs/images-design.md`)
- [ ] Font licenses permit commercial use and app distribution
- [ ] Rights are owned or licensed for all audio, BGM, and sound effects used in the app
- [ ] Appropriate credit attribution exists for any third-party creative works used
- [ ] No unauthorized copyrighted material in screenshots or demo videos

**4.3 Trademarks**
- [ ] App name and logo do not infringe any registered trademarks of third parties
- [ ] No unauthorized use of third-party trademarks in the app description or store page
- [ ] No unauthorized use of third-party trademarks in the app UI or text
- [ ] Own brand / logo trademark rights have been confirmed (if applicable)

**4.4 OSS License Compliance**
- [ ] All OSS libraries used in the app are listed
- [ ] License type of each OSS library is confirmed (MIT / Apache 2.0 / GPL, etc.)
- [ ] All attribution obligations are met (copyright notice, full license text, NOTICE file, etc.)
- [ ] Copyleft license obligations (GPL / LGPL, etc.) are confirmed and addressed (if applicable)
- [ ] An OSS license list screen exists within the app
- [ ] OSS license screen is accessible from the Settings screen (or equivalent)
- [ ] Reachable within 2 taps from the main screen
- [ ] License display content matches the actual versions and libraries in use

### Category 5: Functionality and Stability
- [ ] No crashes or freezes (verified in debug build)
- [ ] All screens function correctly
- [ ] Network errors and offline behavior are handled appropriately
- [ ] App functions correctly after returning from background to foreground
- [ ] Deep link and notification tap navigation works correctly (if implemented)

### Category 6: Platform-Specific Requirements

**iOS**
- [ ] App Transport Security (ATS) settings are appropriate
- [ ] Minimum supported iOS version (Deployment Target) is set
- [ ] Supported devices (iPhone / iPad) are correctly configured
- [ ] Face ID / Touch ID usage description is defined (if applicable)

**Android**
- [ ] Target SDK Version meets the latest requirements
- [ ] 64-bit support is complete
- [ ] ProGuard / R8 obfuscation is correctly configured
- [ ] The exported attribute in AndroidManifest.xml is appropriately set

## Execution Order (mandatory — no skipping)

1. Confirm preconditions (Phase 10 complete + debug build success + all tests pass)
2. Create the initial structure of `@docs/store-submission.md`
3. Audit Category 1 (Build and Release Configuration)
4. Audit Category 2 (App Metadata)

### SubAgent Usage ①: After Category 2 is complete
→ Launch `app-store-optimizer` to evaluate:
  - Whether the app metadata contains elements likely to cause issues in App Store or Google Play review
  - Whether there are high-risk rejection points from a Category 4.1 (Store Guidelines) perspective
  - Whether there are consistency issues between the app description and actual functionality
→ Present the evaluation results to the user and resolve any issues before moving to Category 3.

5. Audit Category 3 (Permissions, Privacy, and Legal)

### SubAgent Usage ②: After Category 3 is complete
→ Launch `legal-compliance-checker` to verify:
  - Whether the Privacy Policy and Terms of Service meet App Store and Google Play requirements
  - Whether there are any easily overlooked legal risks in Category 4 (copyright, trademarks, OSS licenses)
  - Whether the access path to each legal document meets the requirement (within 2 taps)
→ Present the verification results to the user and resolve any issues before moving to Category 4.

6. Audit Category 4 (Store Policy, Copyright, Trademarks, OSS)
7. Audit Category 5 (Functionality and Stability)
8. Audit Category 6 (Platform-Specific Requirements)
9. Implement fixes for all identified issues

### SubAgent Usage ③: After fix implementation is complete (after step 9)
→ Launch `test-results-analyzer` to analyze:
  - Whether there are any regressions across all Phase 3, 4, 5, and 8 test results
  - Whether any tests were affected by the Phase 11 fixes
→ Present the analysis results to the user before proceeding to Completion Criteria verification.

10. Document user-action items in `@docs/store-submission.md` Section 4
11. Present the Completion Criteria checklist to the user
12. **Instruct the user to create the release build** (see Release Build Instruction Rules below)
13. After user confirms release build success → complete `@docs/store-submission.md` and request final approval

After each category audit:
- Report issues using the Issue Report Format
- Implement fixes for items ClaudeCode can resolve
- Document user-action items in `@docs/store-submission.md` Section 4
- Obtain user confirmation before moving to the next category

## Release Build Instruction Rules (MANDATORY)
After all Category 1–6 audits and fixes are complete, ClaudeCode must:

1. Confirm that the Completion Criteria checklist is fully satisfied (excluding the release build item)
2. Provide the following step-by-step guide to the user:
   - iOS: Archive in Xcode → export IPA with Distribution certificate
   - Android: Generate signed APK / AAB in Android Studio with release Keystore
3. Ask the user to confirm:
   - iOS release build completed successfully
   - Android release build completed successfully
   - App launches correctly from the release build on both platforms
4. Do not mark Phase 11 as complete without explicit user confirmation of all the above
5. If the user reports a release build failure → treat as Critical, identify the cause, fix it, and ask the user to retry

## Issue Report Format
If an issue is found: STOP immediately and report in the following format:

```
- **Category**: Build / Metadata / Privacy / Terms / Legal / Policy / Copyright / Trademark / OSS / Stability / Platform
- **Platform**: iOS / Android / Both
- **Issue**:
- **Action**: Fix by ClaudeCode / Manual action required by user
- **Priority**: Critical (risk of review rejection) / Recommended (quality improvement)
```

Do not proceed to the next category until the user resolves or approves the reported issue.

## Fix Rules
When implementing fixes:
- Apply only minimal, targeted fixes
- No refactoring of unrelated code
- No addition of new features
- After any code change, re-run all Phase 3, 4, 5, and 8 tests to confirm no regression
- Present fix evidence (build success, test results, or observed behavior)

## Document Template (structure must not be changed)
```
# Store Submission Readiness Document

## 1. App Information
### 1.1 App Name
### 1.2 Version / Build Number
### 1.3 Target Platforms

## 2. Review Checklist
### 2.1 Build and Release Configuration
### 2.2 App Metadata
### 2.3 Permissions, Privacy, and Legal Accessibility
#### 2.3.1 Permissions
#### 2.3.2 Privacy Policy
#### 2.3.3 Terms of Service
#### 2.3.4 In-App Legal Link Verification
### 2.4 Store Policy and Guidelines Compliance
#### 2.4.1 Store Guidelines Compliance
#### 2.4.2 Copyright and Asset Licenses
#### 2.4.3 Trademarks
#### 2.4.4 OSS License Compliance
### 2.5 Functionality and Stability
### 2.6 Platform-Specific Requirements (iOS)
### 2.7 Platform-Specific Requirements (Android)

## 3. Issues Found and Resolution Status
| # | Category | Issue | Priority | Status | Resolution |

## 4. Items Requiring Manual Action by User
### 4.1 App Store Connect Action Items
### 4.2 Google Play Console Action Items

## 5. OSS License List
| Library Name | Version | License Type | Attribution Compliance Status |

## 6. Final Confirmation
### 6.1 Release Build Confirmation (iOS)
### 6.2 Release Build Confirmation (Android)
### 6.3 Approval
```

## Completion Criteria (confirm all items before instructing the release build)

- [ ] Category 1 (Build and Release): All items resolved
- [ ] Category 2 (Metadata): All items resolved, cross-checked with `@docs/images-design.md`
- [ ] Category 3.1 (Permissions): All items resolved
- [ ] Category 3.2 (Privacy Policy): All items resolved, reachable within 2 taps from main screen
- [ ] Category 3.3 (Terms of Service): All items resolved, reachable within 2 taps from main screen
- [ ] Category 3.4 (In-App Legal Links): All links verified in the Settings screen
- [ ] Category 4.1 (Store Guidelines): All items resolved
- [ ] Category 4.2 (Copyright and Asset Licenses): All items resolved, cross-checked with `@docs/images-design.md`
- [ ] Category 4.3 (Trademarks): All items resolved
- [ ] Category 4.4 (OSS License Compliance): All items resolved, reachable within 2 taps from main screen
- [ ] Category 5 (Functionality and Stability): All items resolved
- [ ] Category 6 (Platform-Specific): All items resolved
- [ ] OSS license list documented in `@docs/store-submission.md` Section 5
- [ ] Phase 3, 4, 5, and 8 tests all pass (no regression)
- [ ] User-action items clearly documented in `@docs/store-submission.md` Section 4
- [ ] `@docs/store-submission.md` is complete (Section 6 to be filled after release build)
- [ ] `app-store-optimizer` review risk evaluation completed
- [ ] `legal-compliance-checker` legal compliance verification completed
- [ ] `test-results-analyzer` Final Regression analysis completed
- [ ] User has confirmed successful release build on both iOS and Android platforms

## Exit Criteria (Phase 11)

Mark Phase 11 as complete only when ALL of the following are satisfied:

- [ ] All audit checklist items are resolved or documented as user-action items
- [ ] Privacy Policy, Terms of Service, and OSS license screen are all accessible within the app
- [ ] No known policy violations remain
- [ ] Phase 3, 4, 5, and 8 tests all pass (no regression)
- [ ] User has confirmed successful release build on both iOS and Android platforms
- [ ] `@docs/store-submission.md` Section 6 is filled with release build confirmation
- [ ] `@docs/store-submission.md` is complete and user-approved
- [ ] User has explicitly confirmed Phase 11 completion and submission readiness

## Start
First confirm that Phase 10 is complete and all tests pass,
then create the initial structure of `@docs/store-submission.md`,
plan the Category 1 (Build and Release Configuration) audit in plan mode, and propose it to the user.
Obtain user approval before starting the audit.