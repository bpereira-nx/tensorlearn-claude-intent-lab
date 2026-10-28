# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18) and `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` is superseded by PRD N-17 and is history only: nothing in this intent is approved on its authority.

## Problem

- **P-1** The clinic receives avoidable calls from patients checking whether their results are ready (01: "avoidable calls from patients checking whether results are ready").

## Proposed outcome

- **I-1** Notify the patient when a result is ready for their review, and direct them to the existing authenticated portal to see it (01).
- **I-2** A result counts as ready only after a doctor has reviewed and released it. A completed lab upload is not enough on its own (01: "A doctor must review and release the result before any notification is eligible"; 02, product owner: "'Ready' must not mean that the laboratory upload merely finished").
- **I-3** The notification says only that an update is available and points to the existing authenticated portal (01).

### How success will be observed

- **S-1** Fewer **status-only calls** (patients asking whether results are ready), counted separately from clinical questions and compared with a measured baseline (01; 02, support lead: "count status-only calls separately from clinical questions; compare against a measured baseline").
- **S-2** **Delivery failures** of notifications are measured (01: "Measure reduction in results-status calls and delivery failures").
- **S-3** No numeric target is approved. The baseline and any target will be agreed during discovery (01: "none is approved yet"), and the discovery notes do not set one (see OPEN-3). The superseded "90% call reduction in one week" is not a target.

## Affected users and systems

- **U-1** Patients: the only approved recipients of the notification.
- **U-2** Doctors: they review and release results, which is the gate for every notification.
- **U-3** Clinic support staff: they handle the calls this feature should reduce, and they count status-only calls separately from clinical questions.
- **U-4** Owners: product owner, privacy owner, clinical owner, engineering lead and support lead (01, 02).
- **SYS-1** The existing authenticated patient portal and its existing result route. The engineering lead says this path is adequate (02).
- **SYS-2** The result review and release workflow, which provides the notification trigger.
- **SYS-3** The notification delivery mechanism and its logs. The channel is still open (OPEN-1).

## Scope

- **IN-1** Notify the patient through the existing portal result route (01: "Use the existing portal result route").
- **IN-2** Duplicate prevention: no repeat notifications for the same released result version (01).
- **IN-3** Delivery-failure handling that never produces duplicate patient messages (01).

## Out of scope

- **X-1** Portal redesign (01).
- **X-2** A new or rebuilt patient dashboard. This is a separate project (02, engineering lead), and the request for it in 03 is superseded.
- **X-3** Appointment scheduling (01).
- **X-4** Clinical interpretation of results (01).
- **X-5** Notifying third parties (01). The referring doctor is not approved as a recipient and is kept as a separate open decision (OPEN-2).
- **X-6** Everything else that only the superseded request asks for: sending on lab upload, including the test result in an SMS, copying the referring doctor, and the 90%-in-one-week target.

## Constraints

- **C-1** Review before notification: no notification is eligible until a doctor has reviewed and released the result (01, 02).
- **C-2** No clinical detail anywhere: no diagnosis, test result, clinical measurement or treatment detail in the message, the notification logs or operational logs (01; 02, privacy owner: "no sensitive content in messages or operational logs").
- **C-3** Access stays authenticated in the existing portal. Do not attach results and do not introduce a public result link (01).
- **C-4** At most one notification per released result version, including when delivery is retried after a failure (01).
- **C-5** Recipients are limited to the patient. No other recipient is approved until OPEN-2 is decided (01; 02, clinical owner: "do not infer approval for referring-doctor notifications from the old request").
- **C-6** Consent and channel are still under privacy review. Nothing may assume a channel before OPEN-1 is decided (02).

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner** and **privacy owner**. To be decided after the consent and delivery review (01, 02).
- **OPEN-2 Referring-doctor notification.** Owners: **clinical owner** and **privacy owner**. They must decide whether the recipient is authorized. It is not approved by the PRD and must not be inferred from the superseded request (01, 02).
- **OPEN-3 Baseline and numeric target** for status-only calls. 01 says both "will be agreed during discovery; none is approved yet", and the 2026-09-20 discovery notes do not agree on either. The support lead asks for a measured baseline (02). The sources do not name an owner for this decision. Until it is agreed, success is measured as a change from the measured baseline, with no target (S-1, S-3).

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
