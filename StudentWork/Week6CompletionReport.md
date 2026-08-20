# Week 6 Test Completion Report

## 1. Release and scope

Release/build: AB-1.4-RC3
Scope tested: 17 of the 20 planned tests produced Passed or Failed execution results. Tested scope included booking and validation, slot-state integrity, duplicate prevention, concurrency, cancellation and authorisation, database persistence and rollback, SMS reminder and provider-failure behaviour, end-to-end booking, and failure-message usability.
Scope not tested or blocked: TC-013 was Blocked because the required restart window was not made available. TC-018 (end-to-end cancellation) and TC-020 (audit-log verification) were Not Run because they were deferred at the deadline.

## 2. Final results

Definitions and calculations:

- Planned = all 20 tests in the test portfolio.
- Executed = tests with a final Passed or Failed result. Blocked and Not Run tests are not counted as Executed.
- Passed = executed tests where the expected result was observed.
- Blocked = tests that could not be meaningfully executed because a required condition or dependency was unavailable.

Final status counts:
- Passed: 15
- Failed: 2
- Blocked: 1
- Not Run: 2
- Executed: 17

Final metrics:
- Execution progress = 17 executed / 20 planned × 100 = 85%
- Pass rate = 15 passed / 17 executed × 100 = 88.2%
- Blocked proportion = 1 blocked / 20 planned × 100 = 5%
- High/Critical coverage = 11 executed High/Critical tests / 13 planned High/Critical tests × 100 = 84.6%

The 88.2% pass rate does not by itself demonstrate release readiness. TC-008 and TC-016 remain failed, while TC-013 is blocked and TC-018 and TC-020 were not run.

## 3. Deviations from the cycle addendum

Several deviations occurred during the test cycle:

- SMS testing was delayed because the SMS sandbox was unavailable at Checkpoint A. TC-015 and TC-016 were initially blocked and were executed later after the sandbox recovered. TC-015 then passed, while TC-016 exposed a product resilience defect.
- Additional test-fixture investigation and correction were required for TC-010. The original suite-only failure was associated with a shared static appointment fixture under parallel execution. After a separate appointment fixture was provided for each test, the verification passed.
- Additional regression work was required after the DEF-001 duplicate-booking correction. Although the original defect passed confirmation, regression testing exposed an over-broad duplicate response in TC-017, requiring further correction before TC-017 passed on AB-1.4-RC3.
- The planned test scope was not fully completed by the deadline. TC-013 remained blocked because the required restart window was unavailable, while TC-018 and TC-020 were deferred and remained Not Run.

These deviations changed the planned sequence and use of tester, developer and environment capacity and left some planned evidence unavailable at cycle completion.

## 4. Exit-criteria assessment

| Exit criterion | Met / Not met | Evidence | Decision consequence |
|---|---|---|---|
| 1. All Critical-risk tests (TC-008, TC-011 and TC-014) have been executed with reliable evidence. | Met | TC-008 was executed and Failed, while TC-011 and TC-014 Passed on AB-1.4-RC3. All three Critical-risk tests therefore produced execution evidence. | Critical-risk coverage is complete, but the TC-008 failure remains a residual risk that must affect the release decision. |
| 2. No unresolved Critical or Severity 1 product defect remains before the supervised pilot decision. | Met | The supplied evidence does not classify any remaining product defect as Severity 1. TC-008 is a Critical-risk test failure, but the available evidence does not establish that the associated defect is Severity 1. | This criterion does not itself prevent the pilot, but unresolved failures such as TC-008 and TC-016 still require explicit risk assessment and mitigation. |
| 3. Important state-integrity and authorisation tests, including TC-006, TC-009, TC-010, TC-011 and TC-014, pass with no unresolved evidence of state corruption or unauthorised cancellation. | Met | TC-006, TC-009, TC-010, TC-011 and TC-014 all Passed on the final evidence. | The specified state-integrity and authorisation scenarios support the pilot decision. |
| 4. Any blocked or untested scope is explicitly documented with its residual risk, mitigation, owner and required follow-up before the release decision. | Met | TC-013 is Blocked and TC-018 and TC-020 are Not Run. These gaps are identified for explicit residual-risk treatment in the completion report. | Release can only be recommended after these evidence gaps and their mitigations, owners and follow-up actions are made visible to the decision-maker. |

## 5. Remaining defects and residual risks

| Defect or gap | Exposure and impact | Mitigation | Risk owner | Follow-up |
|---|---|---|---|---|
| TC-008 failed — concurrent final-slot requests can still produce an unhandled exception. | Under concurrent booking attempts, an API consumer may receive an uncontrolled server error. The scenario is Critical-risk, although the available evidence did not show more than one booking being persisted. | Restrict or closely supervise concurrent final-slot booking during the pilot and make the known concurrency risk visible to pilot stakeholders. | Developer / Release owner | Investigate and correct the concurrency failure, then rerun TC-008 and related booking regression tests. |
| TC-016 failed — an SMS provider error rolls back a valid booking. | Failure of the external reminder provider can undo an otherwise valid booking, creating a High-risk resilience and booking-state impact. | Treat SMS-provider failure as a known pilot restriction and closely monitor booking state when reminder delivery fails. | Developer / Release owner | Correct the transaction/error-handling behaviour and rerun TC-016 plus targeted booking and persistence regression tests. |
| TC-013 blocked — restart persistence was not verified. | Reliability after a service restart remains unverified; there is insufficient evidence to claim that saved bookings reliably survive restart. | Avoid treating restart persistence as proven and arrange a controlled restart window before broader release. | Environment specialist / Test lead | Make a restart window available and execute TC-013. |
| TC-018 not run — end-to-end cancellation was not verified. | Although component/service cancellation evidence passed, the complete UI/API-to-database cancellation path remains unverified. | Keep cancellation within the supervised pilot scope only with monitoring and make the evidence gap visible to the pilot owner. | Test lead / Pilot owner | Execute TC-018 at the earliest available opportunity and investigate any discrepancy. |
| TC-020 not run — final audit-log behaviour was not verified. | Final evidence does not demonstrate that booking and cancellation actions are recorded with the required actor, action and time without unnecessary patient data. | Treat auditability as an explicit evidence gap and monitor/review audit records during the supervised pilot where possible. | Test lead / Pilot owner | Execute TC-020 before broader release and address any auditability failure found. |

## 6. Release recommendation

Recommendation: restricted release

Evidence-based rationale:
AB-1.4-RC3 should proceed only as a restricted supervised pilot rather than a full release. Fifteen tests passed and all three Critical-risk tests were executed, with the important authorisation and state-integrity scenarios identified in the exit criteria passing. Previously identified duplicate-booking, unauthorised-cancellation and over-broad duplicate-key behaviours also passed their final confirmation or regression evidence.
However, the remaining evidence does not support a full release. TC-008 remains failed because an unhandled exception is still possible during concurrent final-slot requests, and TC-016 remains failed because an SMS provider error can roll back a valid booking. In addition, TC-013 is blocked and TC-018 and TC-020 were not run, leaving persistence-after-restart, end-to-end cancellation and final audit-log evidence incomplete.
A restricted supervised pilot is therefore recommended only if these residual risks and evidence gaps are explicitly accepted, owned and monitored.

Restrictions or conditions, if any:

- The pilot must remain supervised and limited in scope rather than progressing to a full release.
- The known TC-008 concurrency risk must be communicated to the pilot owner, with concurrent final-slot booking closely supervised and the defect remaining open for correction and retest.
- The TC-016 SMS resilience defect must remain open. SMS-provider failures and their effect on booking state must be monitored during the pilot until the defect is corrected and retested.
- TC-013 must be executed when a controlled restart window becomes available.
- TC-018 and TC-020 must be executed as follow-up tests before a broader release decision.
- Any unexpected booking-state, cancellation or audit behaviour observed during the pilot should trigger reassessment of the release decision.

## 7. Copilot challenge and human judgement

Prompt used:
I asked Copilot to challenge the restricted-release recommendation using only the supplied final evidence, identifying one reason why the recommendation might be too permissive and one reason why it might be too conservative, while clearly distinguishing evidence from assumptions.

Useful challenge accepted:
Copilot challenged that even a restricted supervised pilot may be too permissive because TC-008 remains a Critical-risk failure and TC-016 remains a High-risk failure. I accepted this challenge because supervision does not remove the underlying product failures. This reinforced the need to keep both defects open, explicitly communicate them to the pilot owner, and maintain the proposed restrictions and follow-up testing.

Suggestion rejected or modified:
Copilot suggested that a restricted release might be too conservative because 15 of 17 executed tests passed, two of the three Critical-risk tests passed, important state-integrity and authorisation tests passed, and several earlier defects were confirmed corrected. I accepted this as useful positive evidence but did not accept it as sufficient justification for a less restricted release.

Why human judgement was required:
The overall pass rate and successful confirmation results had to be weighed against the nature of the remaining evidence. TC-008 still exposes a Critical-risk concurrency failure, TC-016 can roll back a valid booking when the SMS provider fails, TC-013 remains blocked, and TC-018 and TC-020 were not run. Copilot also explicitly noted that assuming supervision and monitoring can adequately control these risks would require operational evidence that was not provided. Human judgement was therefore required to avoid treating the high pass rate as sufficient evidence for a broader release and to retain the restricted supervised-pilot recommendation.
