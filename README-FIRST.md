# ENSE707 Week 6 Lab Pack

## Managing a Test Cycle: From Readiness to Release Decision

This pack supports a test-management lab based on the familiar AppointmentBooking domain. The production solution is used only to establish a reproducible baseline. The wider release-candidate evidence is supplied in `LabData` so that every student can complete the management activities even if their earlier optional implementations differ.

## Contents

- `ENSE707_Week6_Lab_Test_Management.docx` - student lab handout.
- `Starter` - clean AppointmentBooking class library and MSTest project.
- `LabData` - release brief, test portfolio, estimation inputs, execution snapshots and anomaly evidence.
- `Templates` - editable templates for the required student artefacts.

## Start here

1. Open `Starter/AppointmentBooking.slnx` in Visual Studio, or open the `Starter` folder in VS Code.
2. Restore packages and run the baseline tests.
3. Create a branch named `week6-test-management` in your own GitHub repository.
4. Create a `StudentWork` folder and copy the templates into it before editing them.
5. Keep the files in `LabData` unchanged so that your calculations and decisions remain traceable to the supplied evidence.

Command-line option:

```text
dotnet restore Starter/AppointmentBooking.slnx
dotnet test Starter/AppointmentBooking.slnx
```

## Important distinction

The baseline solution should build and its existing tests should pass. The supplied release evidence intentionally includes failed, blocked and unexecuted tests. These are not setup errors: they are the evidence students must investigate and manage.
