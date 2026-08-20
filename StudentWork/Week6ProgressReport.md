# Week 6 Test Progress Report

## Reporting point

Release/build: AB-1.4-RC1
Evidence source: LabData/CheckpointA.csv and LabData/AnomalyEvidence.md
Definitions used for planned, executed, passed and blocked: Planned = all 20 tests in the test portfolio; Executed = tests with a Passed or Failed result; Passed = executed tests where the expected result was observed; Blocked = tests that could not be meaningfully executed because a required dependency was unavailable.

## Metrics

| Metric | Formula and values | Result | Interpretation |
|---|---|---:|---|
| Execution progress | 15 executed / 20 planned × 100 | 75% | Three-quarters of the planned portfolio has produced an execution result; five tests remain blocked or not run. |
| Pass rate | 11 passed / 15 executed × 100 | 73.3% | ... |
| Blocked proportion | 2 blocked / 20 planned × 100 | 10% | ... |
| High/Critical coverage | 9 executed High/Critical tests / 13 planned High/Critical tests × 100 | 69.2% | ... |

Note: If a reporting tool counted Blocked tests as Executed, execution progress would appear as 17/20 = 85% rather than 75%, because the two blocked SMS tests would be included even though they did not produce a Passed or Failed execution result.

## Status and forecast

Important evidence: Critical authorisation test TC-011 failed because one user could cancel another user's appointment. Critical concurrency test TC-008 also failed due to intermittent unhandled exceptions, although no duplicate booking was created. High-risk duplicate-prevention test TC-007 failed reproducibly. The TC-010 failure is currently classified as a test-fixture problem rather than a confirmed product defect. Critical data-integrity test TC-014 has not yet been executed.
Main blockers: TC-015 and TC-016 are blocked because the SMS sandbox cannot be resolved from staging. Recovery time is unknown. TC-013 and TC-014 also remain not run because their required restart window and failure-injection setup were not yet available at Checkpoint A.
Forecast against the plan: At risk. Although 75% of the planned tests have been executed, Critical and High-risk evidence remains incomplete and confirmed product defects require investigation and retest. The unknown SMS sandbox recovery time may also prevent completion of the planned SMS scope before the release decision.

## Control actions

| Action | Signal that triggered it | Expected benefit | Trade-off or new risk | Owner |
|---|---|---|---|---|
| Prioritise investigation and correction of the TC-011 authorisation defect, followed by targeted retesting. | Critical authorisation test TC-011 failed and another user was able to cancel an appointment they did not own. | Addresses a Critical security risk and provides evidence needed for the pilot decision. | Developer time is limited to two hours, so prioritising TC-011 may delay investigation of other defects. | Developer |
| Prioritise preparation of the failure-injection setup and execute TC-014 as soon as the setup is ready. | Critical data-integrity test TC-014 remains Not Run because its required setup was not prepared. | Closes a major Critical-risk evidence gap before the release decision. | Preparing the specialised setup consumes limited environment/tester time and may delay lower-risk tests. | Environment specialist / Tester |
| Continue non-SMS testing while keeping TC-015 and TC-016 blocked, and recheck the SMS dependency when recovery is reported. | The SMS sandbox outage blocks TC-015 and TC-016, while the staging application and database remain available. | Uses limited tester time on executable scope instead of waiting for an external dependency. | SMS evidence may remain incomplete if the sandbox is not restored before the release decision. | Testers / Environment specialist |

## Communication required

The clinic decision-maker should be informed that 75% execution progress does not indicate release readiness. Critical authorisation and concurrency tests have failed, Critical data-integrity evidence is still missing, and SMS-dependent testing remains blocked. The developer should receive the confirmed application-defect evidence for investigation, while the environment specialist should continue tracking the SMS outage and prepare the TC-014 failure-injection environment. Any unresolved Critical risk or blocked scope must be visible in the final release recommendation.

