# Anomaly Classification Notes

## ANO-01 — Duplicate booking after a retry

Classification: Application defect
Evidence: The same booking body and request key produced two separate booking records, decreased the slot count twice, and the behaviour was reproduced in three out of three controlled retries.
Remaining unknown: The underlying cause of the duplicate creation within the application has not yet been identified.

## ANO-02 — Concurrent final-slot request

Classification: Application defect
Evidence: In three of ten concurrent final-slot runs, one request produced an unhandled InvalidOperationException, although the database never contained more than one booking.
Remaining unknown: The underlying cause of the intermittent InvalidOperationException has not yet been identified, and no application log correlation ID was returned to support further tracing.

## ANO-03 — Repeat cancellation fails only in the suite

Classification: Test asset or fixture problem
Evidence: TC-010 failed during the full suite but passed five isolated reruns, while the cancellation tests share the same static appointment object and method-level parallel execution is enabled.
Remaining unknown: It has not yet been confirmed whether the shared test fixture fully explains the failure, and the production cancellation service has not yet been ruled out as a contributing cause.

## ANO-04 — Another user's appointment can be cancelled

Classification: Application defect
Evidence: An authenticated user was able to cancel another user's appointment, receiving HTTP 204 instead of the expected 403, and the behaviour was reproduced with two different user pairs.
Remaining unknown: The underlying cause of the authorisation failure has not yet been identified.

## ANO-05 — SMS tests blocked

Classification: Environment/dependency incident
Evidence: DNS resolution for the SMS sandbox failed from both staging and the environment specialist's diagnostic container, and the provider reported a sandbox outage; no product request reached the SMS adapter.
Remaining unknown: The SMS sandbox recovery time is unknown, and the product's reminder behaviour cannot yet be evaluated until the dependency becomes available.

## Copilot reflection

I used GitHub Copilot to draft the ANO-04 defect report using the supplied anomaly evidence. I reviewed the draft against the provided evidence before using it.
Copilot suggested verifying database fields such as `cancelled_by` and `cancelled_at`. This was an unsupported assumption because the supplied evidence does not state that these fields exist in the database schema.
I removed these field names and kept the report limited to the evidence that the appointment status became cancelled after the unauthorised DELETE request.