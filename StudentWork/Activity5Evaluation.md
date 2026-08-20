# Activity 5 - Confirmation, Regression and Workflow Evaluation

## 1. Result classification

| Result | Test | Purpose | Status |
|---|---|---|---|
| FR-01 | TC-007 | Confirmation | Passed |
| FR-02 | TC-001 | Regression | Passed |
| FR-03 | TC-017 | Regression | Failed |
| FR-04 | TC-011 | Confirmation | Passed |
| FR-05 | TC-015 | Previously blocked | Passed |
| FR-06 | TC-016 | Previously blocked | Failed |
| FR-07 | TC-010 | Test-fixture verification | Passed |

## 2. DEF-001 confirmation and regression

FR-01 provides confirmation evidence that the original DEF-001 duplicate-booking failure is fixed in AB-1.4-RC2. Retrying the same booking request returned the original booking and did not reserve another slot.
However, FR-03 reveals a regression. A different patient attempting to book the same doctor and time received HTTP 409 as if the request were a duplicate. This shows that although the original retry problem passed confirmation, the change has affected other booking behaviour that previously worked.

## 3. DEF-001 workflow decision

DEF-001 should be accompanied by a new linked regression issue. FR-01 passed and provides evidence that the original duplicate-retry failure has been corrected in AB-1.4-RC2. However, FR-03 failed and revealed a different booking failure: a different patient's booking for the same doctor and time was incorrectly rejected with HTTP 409 as if it were a duplicate.
Tracking FR-03 as a new linked regression issue preserves the distinction between the original defect, which passed confirmation, and the newly observed regression. DEF-001 should only be closed with the FR-01 confirmation evidence recorded and the new regression issue linked to it.

## 4. ENV-001 sandbox recovery

Restoration of the SMS sandbox removed the environmental blocker and allowed the previously blocked SMS tests to be executed. FR-05 passed, showing that one reminder could be sent successfully after the sandbox recovered.
However, sandbox recovery does not prove that the product handles SMS provider failure correctly. FR-06 failed because a provider error caused the booking transaction to be rolled back, whereas TC-016 requires the booking to remain confirmed and the reminder failure to be recorded. Therefore, ENV-001 can be considered resolved as an environment incident, but FR-06 provides evidence of a separate product defect that requires investigation.

## 5. TEST-001 fixture verification

FR-07 passed after each cancellation test was given a new appointment fixture instead of sharing the same static appointment object. This removed the suite-only TC-010 failure.
The fixture change improves evidence reliability because the tests no longer share mutable appointment state during parallel execution. This reduces the risk that one test affects the state observed by another test and supports the earlier classification of ANO-03 as a test-fixture problem rather than a confirmed application defect.
FR-07 demonstrates that TC-010 passes with the corrected fixture, but it does not prove that all cancellation behaviour is defect-free.

## 6. Targeted regression scope

The targeted regression scope should focus on booking creation, duplicate handling, persistence and SMS-failure resilience rather than rerunning the entire portfolio.

- TC-007 should be retained to confirm that retry protection continues to prevent duplicate bookings.
- TC-001 should be retained as a regression check that a normal first booking still succeeds.
- TC-008 should be rerun because it is a Critical concurrency scenario and previously produced intermittent unhandled exceptions.
- TC-012 should be rerun to verify that booking persistence remains correct after changes affecting booking transactions.
- TC-016 should be retested after correction of the newly identified SMS-failure defect to verify that provider failure does not roll back a valid booking.
- TC-017 should be rerun because it exposed the new HTTP 409 regression and provides end-to-end evidence across the booking and persistence path.

This scope is risk-based: it concentrates limited tester and developer time on the components and behaviours affected by the fixes, the newly observed regressions, and Critical or High-risk booking-state scenarios.

## 7. Issue updates

- The existing authorisation defect issue should be updated with FR-04 confirmation evidence. In AB-1.4-RC2, TC-011 passed: a non-owner received HTTP 403 and the appointment remained active.
- The SMS environment incident should be updated to show that the sandbox recovered. FR-05 demonstrates that TC-015 could be executed and one reminder was successfully sent. The environment incident can therefore be resolved, but this does not resolve the separate product failure identified by FR-06.
- A new regression issue should be created for FR-03 and linked to DEF-001. A different patient booking the same doctor and time was incorrectly rejected with HTTP 409 after the duplicate-retry change.
- A new application-defect issue should be created for FR-06. When the SMS provider returned an error, the booking transaction was rolled back instead of remaining confirmed as required by TC-016.