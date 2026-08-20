# Week 6 Test-Cycle Addendum

## Baseline evidence

Build/commit: 3e53821
Operating system: Windows 10 Home, Version 2009
.NET SDK version: 10.0.400
Test command: dotnet test AppointmentBooking.slnx
Execution date: 20 August 2026
Tests discovered: 12
Tests passed: 12
Tests failed: 0
Tests skipped: 0

### Remaining confidence gaps
1. The passing domain-level tests do not demonstrate that booking and cancellation work correctly with the integrated persistence layer.
2. The baseline tests do not demonstrate that reminder behaviour works correctly with the external messaging dependency in the integrated release environment.

## Release and objective

Release/build: AB-1.4-RC1
Decision this cycle must support: Determine whether AB-1.4-RC1 is suitable for the supervised pilot as a full release, a restricted release with mitigation, or should be delayed.

## Readiness decision

Decision: Start partially

Rationale: AB-1.4-RC1 is deployed to staging, the domain-level baseline passes, required synthetic test data is prepared, and the staging database is available. However, the SMS sandbox is currently unavailable from staging. Testing can therefore begin for scope that does not depend on the SMS sandbox, while SMS-dependent tests TC-015 and TC-016 remain blocked until the dependency is restored.

| Entry criterion | Met / Partly / Not met | Evidence | Consequence |
|---|---|---|---|
| AB-1.4-RC1 is successfully deployed to the staging environment. | Met | AB-1.4-RC1 is deployed to staging. | Testing can proceed against the intended release candidate. |
| The domain-level baseline passes and the required synthetic test data and accounts are prepared. | Met | The domain-level MSTest baseline passes, and test users, doctors and appointments have been prepared. | Domain-level baseline and prepared test data support meaningful execution of the applicable tests. |
| The staging database is available and passes the environment smoke check. | Met | The test database responds to the environment smoke check. | Database-dependent testing can begin. |
| External dependencies required by the planned test scope are available and reachable from staging. | Partly met | The SMS sandbox does not currently resolve from staging, and recovery time is unconfirmed. | Non-SMS testing may proceed, but TC-015 and TC-016 cannot currently produce reliable execution evidence. |

## Exit criteria

1. All Critical-risk tests (TC-008, TC-011 and TC-014) have been executed with reliable evidence.
2. No unresolved Critical or Severity 1 product defect remains before the supervised pilot decision.
3. Important state-integrity and authorisation tests, including TC-006, TC-009, TC-010, TC-011 and TC-014, pass with no unresolved evidence of state corruption or unauthorised cancellation.
4. Any blocked or untested scope is explicitly documented with its residual risk, mitigation, owner and required follow-up before the release decision.

## Suspension and resumption

Suspension condition: Suspend affected integration and system testing if the staging application or database becomes unavailable or unstable such that reliable evidence cannot be produced for Critical or High-risk tests.
Resumption evidence required: The affected staging service or database is restored and a repeat environment smoke check passes, demonstrating that the environment is stable enough to produce reliable test evidence.

## Work-breakdown estimate

| Work item | Effort | Dependency | Can run in parallel? | Assumption |
|---|---:|---|---|---|
| Environment smoke checks | 30 min | Staging build and accounts | Partly | Application and database checks can be split between available staff. |
| Prepare and verify test-data sets | 60 min | Environment smoke checks | Yes | Four synthetic test-data sets can be prepared independently once the environment is confirmed. |
| Execute planned test cases | 240 min | Environment and test data | Yes | The 20 tests average 12 minutes each; tests with unavailable dependencies may be blocked. |
| Investigate and triage anomalies | 100 min | Initial test results and evidence | Partly | Four anomalies are assumed; the developer joins after initial classification. |
| Confirmation and targeted regression tests | 90 min | Resolved build | Yes | Six tests are assumed; the exact regression scope may change after triage. |
| Prepare progress report | 30 min | Checkpoint A evidence | No | Reporting can begin while anomaly investigation continues. |
| Prepare completion report | 45 min | Final evidence and decisions | No | Final results, residual risks and release decision are available. |

Total estimated person-hours: 9 hours 55 minutes.
Estimated calendar duration: Approximately one working day, assuming parallel execution by Tester A and Tester B, timely developer availability after triage, and no extended environment or SMS recovery delay.
Main uncertainty: The recovery time of the SMS sandbox, because an extended outage may keep TC-015 and TC-016 blocked and change the achievable scope and release evidence.
