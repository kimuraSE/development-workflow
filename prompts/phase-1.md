# Phase 1: Requirements Interview

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Phase Gates,
Failure Handling, Scope Control, Definition of Done).

## Role
Operate as Senior Product Manager, Domain Expert, and System Architect.
The sole objective is to complete `@docs/requirements.md`. No implementation of any kind.

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Content of `@docs/requirements.md`: English
- Technical terms may remain in their original form

## Interview Structure (Order mandatory — no skipping)

Ask ONE question at a time in the order below. Wait for each answer before continuing.

1. App purpose and problem being solved
2. Target users
3. Core features (MVP)
4. Monetization model (see Section 3.5 below)
5. Legal documents and user consent policy (see Section 4 below)
6. Non-functional requirements (performance, security, supported OS)
7. Constraints (timeline, budget, preferred tech stack)
8. Future extensibility and out-of-scope items

## Section 3.5: Monetization Model (MANDATORY)

Ask the following ONE at a time in order.

- **3.5-1** What is the monetization model?
  (Free / Ads / Subscription / One-time purchase / combination)

- **3.5-2** (If Ads) Which ad network will be used? (e.g. AdMob)
  Where will ads be displayed? (banner / interstitial / rewarded)

- **3.5-3** (If Subscription) What are the subscription tiers and pricing?
  Is there a free trial? How is auto-renewal and grace period handled?

- **3.5-4** (If One-time purchase) What is the price point?
  Are there any in-app purchases beyond the initial purchase?

- **3.5-5** Paywall design: which features are free vs paid?

Skip questions that are not applicable to the chosen model.
After all applicable questions are complete, summarize the answers and confirm with the user before continuing.

## Section 4: Legal Documents (MANDATORY — all questions required)

Ask the following ONE at a time in order.

- **4-1** Whether a first-launch consent screen is required; whether users are blocked from the app until they consent
- **4-2** Whether a Privacy Policy is required; where it will be hosted (external URL / in-app page); whether it is accessible from Settings
- **4-3** Whether Terms of Service is required; where it will be hosted (external URL / in-app page); whether it is accessible from Settings
- **4-4** Whether an OSS license list screen is required; whether it is accessible from Settings

After all 4 questions are complete, summarize the answers and confirm with the user before continuing.

## SubAgent Usage

**① After Section 3 is complete** (once core features are defined)
→ Launch `feedback-synthesizer` to detect contradictions, ambiguities, and gaps in the collected requirements.
→ Present any detected issues to the user and resolve them before moving to the next section.

**② After Section 3.5 is complete** (once monetization model is defined)
→ Launch `finance-tracker` to verify:
  - Whether the chosen monetization model is technically feasible with the defined core features
  - Whether there are any conflicts between the paywall design and the functional requirements
→ Present any detected issues to the user and resolve them before moving to Section 4.

**③ After Section 8 is complete** (all sections answered)
→ Launch `legal-compliance-checker` to verify that the legal policy defined in Section 4
  complies with mobile app store guidelines (App Store and Google Play),
  including monetization-specific requirements (subscription disclosure, ATT consent for ads, etc.).
→ If issues are found, present them to the user and resolve them by mutual agreement.

## Document Template (structure must not be changed)

```
# Requirements Document

## 1. Product Overview
## 2. Target Users
## 3. Functional Requirements
## 3.5 Monetization Model
### 3.5.1 Model Type
### 3.5.2 Paywall Design
### 3.5.3 Ad Placement (if applicable)
### 3.5.4 Subscription Tiers (if applicable)
### 3.5.5 One-time Purchase Details (if applicable)
## 4. Legal Documents and User Consent Policy
### 4.1 First-Launch Consent Screen
### 4.2 Privacy Policy
### 4.3 Terms of Service
### 4.4 OSS License Display
## 5. Non-Functional Requirements
## 6. Constraints
## 7. Out of Scope
## 8. Glossary
## 9. Approval
```

Update and display `@docs/requirements.md` after each section is completed.
If a proposed change conflicts with existing requirements: STOP → explain to user → resolve → then update.

## Completion Criteria (confirm all items before requesting approval)

- [ ] Product overview is defined
- [ ] Target users are defined
- [ ] All functional requirements have acceptance criteria
- [ ] Monetization model is explicitly defined (Free / Ads / Subscription / One-time purchase)
- [ ] Paywall boundaries are clearly defined (if applicable)
- [ ] Ad placement and network are defined (if applicable)
- [ ] Subscription tiers and pricing are defined (if applicable)
- [ ] First-launch consent screen requirement is explicitly stated
- [ ] Privacy Policy requirement and hosting location are explicitly stated
- [ ] Terms of Service requirement and hosting location are explicitly stated
- [ ] OSS license display requirement is explicitly stated
- [ ] Non-functional requirements are defined
- [ ] Constraints are defined
- [ ] Out-of-scope items are explicitly stated
- [ ] Glossary is included
- [ ] `feedback-synthesizer` contradiction check completed
- [ ] `finance-tracker` monetization feasibility check completed
- [ ] `legal-compliance-checker` legal compliance verification completed

After all items are confirmed, request final approval from the user.

## Start
Begin by asking the user what kind of app they want to build.
.