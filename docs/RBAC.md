# RBAC and authorization matrix

All authorization is enforced in the local application service, not only in UI/router/preload. Matrix defines defaults, not a fixed cap on custom roles. Admin has every registered permission; no implicit trust in client claims.

Symbols: ✓ default granted, — not granted. D=Doctor, R=Receptionist, A=Accountant. Active administrator protections prevent locking out the last administrator. User creation/role changes must be checked for privilege escalation; only security-role administrators may grant privileged capabilities.

| Permission key | Admin | D | R | A | Business scope |
|---|---|---|---|---|---|
| patients.view | ✓ | ✓ | ✓ | ✓ | Identity/contact, not clinical/financial metadata |
| patients.create | ✓ | ✓ | ✓ | — | Register and duplicate acknowledgement |
| patients.edit | ✓ | ✓ | ✓ | — | Identity/contact updates |
| patients.archive | ✓ | — | — | — | Archive/reinstate, retain linked records |
| patients.delete | ✓ | — | — | — | Only protected exceptional deletion |
| clinical.view | ✓ | ✓ | — | — | Visits/chart/history/clinical notes |
| clinical.create | ✓ | ✓ | — | — | New independent visit/referral/note |
| clinical.amend | ✓ | ✓ | — | — | Reasoned amendments, no erasure |
| clinical.chart | ✓ | ✓ | — | — | Append tooth-state events |
| clinical.prescribe | ✓ | ✓ | — | — | Mandatory dentist, medicine entries |
| treatments.view | ✓ | ✓ | ✓ | ✓ | Catalog nonprice data; price gated |
| treatments.manage | ✓ | — | — | — | Catalog/configuration and prices |
| appointments.view | ✓ | ✓ | ✓ | — | Calendar/list/availability |
| appointments.create | ✓ | ✓ | ✓ | — | Enforce conflicts/availability |
| appointments.edit | ✓ | ✓ | ✓ | — | Safe status/reschedule changes |
| appointments.cancel | ✓ | ✓ | ✓ | — | Audit reason/status |
| queue.manage | ✓ | ✓ | ✓ | — | Check-in/call/start/complete/cancel |
| invoices.view | ✓ | — | — | ✓ | Invoice identity/items/money |
| invoices.create | ✓ | — | — | ✓ | Draft/issue |
| invoices.edit | ✓ | — | — | ✓ | Draft only |
| invoices.reverse | ✓ | — | — | — | Reasoned void/credit, reauth |
| payments.view | ✓ | — | — | ✓ | Receipts/allocations |
| payments.create | ✓ | — | — | ✓ | Collect against outstanding |
| payments.reverse | ✓ | — | — | — | Controlled reversal, reauth/reason |
| finance.revenue | ✓ | — | — | ✓ | Financial dashboard/aggregate income |
| finance.expenses | ✓ | — | — | ✓ | Expense detail |
| finance.accounting | ✓ | — | — | ✓ | Ledger and net cash |
| finance.manage | ✓ | — | — | ✓ | Create categorized income/expense |
| finance.reverse | ✓ | — | — | — | Correct ledger without deletion |
| finance.reports | ✓ | — | — | ✓ | Reports and method/dentist totals |
| finance.export | ✓ | — | — | ✓ | Financial CSV/PDF |
| inventory.view | ✓ | ✓ | — | ✓ | Items/batches/stock alerts (price redacted unless financial) |
| inventory.create | ✓ | — | — | ✓ | Items/suppliers/purchases |
| inventory.modify | ✓ | — | — | ✓ | Metadata/authorized stock in/out |
| inventory.adjust | ✓ | — | — | — | Reasoned adjustments/waste/expired/damaged |
| inventory.archive | ✓ | — | — | — | Archive without movement erasure |
| attachments.view | ✓ | ✓ | — | — | Authorized patient document list/read |
| attachments.create | ✓ | ✓ | — | — | Validated local uploads |
| attachments.delete | ✓ | — | — | — | Confirmed removal and audit |
| staff.view | ✓ | — | — | — | Staff ordinary details |
| staff.sensitive | ✓ | — | — | — | Salary/identification/DOB/emergency data |
| staff.manage | ✓ | — | — | — | Create/update/archive profiles |
| users.manage | ✓ | — | — | — | User lifecycle/password reset/revoke |
| security.roles | ✓ | — | — | — | Roles/permission keys; no escalation |
| security.activation | ✓ | — | — | — | Device/admin activation controls |
| settings.manage | ✓ | — | — | — | Clinic/dentists/catalog/security config |
| backup.create | ✓ | — | — | — | Manual/configure automatic backup |
| backup.restore | ✓ | — | — | — | Reauth, typed confirmation, pre-backup |
| audit.view | ✓ | — | — | — | Audit without raw secrets/unauthorized metadata |
| data.export | ✓ | — | — | — | Nonfinancial export, obey clinical/staff scopes |
| data.destroy | ✓ | — | — | — | Protected reset/business delete |

Custom: any deliberately granted combination using this same allowlist. About, current user information, personal appearance, password change and lock are available to authenticated users without extra permission. No guest clinical/financial operations. Admin defaults are seeded without sample users; initial user is explicitly created.

## Projection and query requirements

- Patient identity permissions never imply invoice total/balance/payment details, medical history or attachments. Identity DTO strips nested unauthorized fields, including timeline/search snippets.
- `invoices.view` permits that patient's invoice amounts; `payments.view` permits receipts; aggregate revenue requires `finance.revenue`; broad reports require `finance.reports`; exports require `finance.export`. Check all required permissions, not any one permissive permission.
- Inventory unit costs/supplier purchases/valuation require appropriate financial view permission in addition to inventory access; salary requires `staff.sensitive`. A doctor can see quantities/expiry without acquisition cost.
- Notification queries/search endpoints use the caller's permissions before fetching sensitive rows. Errors cannot reveal a record amount/title through existence checks.
- Role/permission changes loaded fresh per call; deactivation/session resets effective immediately. Reauth applies to restore, reversals, hard delete/reset and sensitive administrative operations. Last admin cannot be removed, deactivated or stripped of administrative abilities.
- Audit metadata is intentionally minimal (IDs, change categories, reason where appropriate), not password/code/token/request body or hidden clinical/financial snapshots. Audit readers must not gain otherwise-denied information.

## Threat tests

For Admin/Doctor/Receptionist/Accountant/custom restricted user: direct unauthorized service call, hidden route, altered UI permissions/localStorage, forged user ID, brute-force attempts, stale session after role update/deactivation, export/print/search/notification bypass, profile/timeline financial leak, cross-window session identity, malicious attachment path. Expect service `FORBIDDEN`/`UNAUTHENTICATED` before DB/file access; no hidden values in responses. Local OS admin tampering is outside the application authorization guarantee.
