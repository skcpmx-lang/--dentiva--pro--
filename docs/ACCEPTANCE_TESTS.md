# Acceptance tests and requirements traceability

**Status:** planning baseline; all cases pending until actual evidence is recorded. A test plan is not a test result. Automated reports and manual observations belong in `docs/TEST_EVIDENCE.md`. Final completion requires the UI, business/database logic, validation/permissions/states/audit/integration/responsiveness/tests/docs criteria, not merely compilation.

## Feature acceptance matrix

| ID | Feature and acceptance criteria | Verification |
|---|---|---|
| SETUP-01 | Empty installation requires activation before setup/any patient access. Wrong/blank/malformed code returns safe feedback; attempt throttling; correct supplied code creates device receipt; restart still activated. No original code in repository/tests/logs/maps. | Unit verifier with unrelated test secret, integration bypass checks, actual packaged activation/restart, static secret scan |
| SETUP-02 | Clinic contact/logo/hours/closed days/BDT/footer saved; two dentists each have multiple qualifications/designations/certifications and independent hours. Initial admin hash only. Failed child write rolls back every setup record; repeat request can't duplicate clinic. | Actual SQLite transaction integration + Playwright wizard, malformed data, restart |
| AUTH-01 | Valid active user login; generic incorrect/inactive failures; no plaintext credentials; unique salt/constant-time verification and bounded throttle. Session bound to transport, logout revokes. | Unit crypto, service adversarial integration, UI |
| AUTH-02 | 5/10/15/30 minutes/authorized disabled lock; server idle expiry; explicit lock/unlock; form survives lock without exposing accessible clinical content. | Clock-controlled integration + UI typing/lock regression |
| RBAC-01 | Administrator/Doctor/Receptionist/Accountant/custom permissions obey matrix for read/write/search/print/export/notifications; changing route/storage/UI or direct hidden service call cannot bypass. Last administrator protected; changed role/deactivation effective immediately. | Direct endpoint/IPC attacks and multi-role E2E |
| SHELL-01 | Correct brand/clinic/Dhaka date/current user/real notifications/actions/lock/menu; grouped collapsible sidebar with focus/tooltip/active/scroll/persisted preference. No inactive fake production routes. | UI manual at all sizes/keyboard/reduced motion |
| DASH-01 | Actual date-range data for appointments/patients/new registrations/queue/stock/expiry/activity. Financial values absent in DOM and responses without permission. Today/7/30/90/year/custom boundaries correctly labeled, real charts and actionable empty state. | Controlled DB aggregate integration, boundary dates, UI projection checks |
| PAT-01 | Register/edit complete identity/contact/emergency/medical/allergy/medication/tags/dentist/language/referral fields; unique patient code; DOB/age/phone validations; likely duplicate warning and deliberate override; no record cap. | Unit validators, SQLite uniqueness/FK/duplicates, UI |
| PAT-02 | Recent-first paged patient list supports all specified advanced filters/sorts/date presets/custom, code/name/Bengali/phone search; efficient large-query plans. | Integration index/query/date filters, 50k performance, UI filters/empty/error/retry |
| PAT-03 | Patient identity and all permitted requested tabs show longitudinal data. New actions preserve patient context. No clinical/financial/staff sensitive fields leak from profile/timeline/search. | Multi-role projection integration/E2E, each profile tab manual |
| VIS-01 | Unlimited independent visits; C/C/O/E/history/examination/diagnosis/procedures/teeth/advice/followup/referral/notes/attachments links. Atomic visit+treatments+Rx+invoice/payment bundle or complete rollback; finalized amendment preserves original. | Integration deliberate failure at every child insertion; workflow/UI |
| CHART-01 | FDI adult/pediatric upper/lower/left/right correctly labelled; single/multi/keyboard selection; configurable conditions; latest/historical state distinguishable, never overwrites prior event. | Tooth-number unit tests, event history integration, clinician/manual UI |
| TRT-01 | Configurable catalog code/category/fee/duration/status/description/notes; optional per-dentist fee; archive retains historical snapshots; unauthorized prices redacted. | Unit money/permissions + catalog/visit integration/UI |
| REF-01 | Patient referrals by/to/doctor/institution/reason/summary/notes/status/followup/outcome; author/date and linked timeline; authorized edits/amendments. | Integration/UI |
| APT-01 | Patient/dentist/date/time/duration/type/reason/status/notes/reminder/source/creator; day/week/month/list/range; enforced availability and overlap, configured authorized override; complete status machine/reschedule history. | Dhaka/leap/midnight unit, concurrent serialized overlap integration, UI |
| QUE-01 | Check-in and daily unique number; patient/appointment/dentist/wait/priority; call next/start/complete/cancel; duplicate active check-in prevented; actual local update. | Queue concurrency/status integration, UI two sessions |
| RX-01 | From module/profile/visit with mandatory dentist; multiple medicines include all specified power/form/strength/quantity/timing/meal/duration/frequency/PRN/instructions; clinical options configurable. | Validation and multi-medicine integration, E2E flows |
| PRINT-01 | Prescription premium clinic/dentist multi-credentials/patient header, C/C/O/E/advice left and meds right when wide; readable narrow reflow; footer/hours and physical blank signature; optional digital signature only configured. | Escaping/template snapshots, actual PDF + printed samples |
| PRINT-02 | Invoice professional clinic header/patient/code/date/number/items/qty/price/discount/tax/total/paid/due/status; no doctor signature/prescription footer. | Reconciliation/template/PDF/physical samples |
| PRINT-03 | A4/A5/mini/thermal/custom, actual preview/printer/profile/print/PDF/cancel; Windows-exposed wireless/Bluetooth/thermal capability only; long addresses/names/20+ medicines/50+ invoice lines no clipping. | Playwright layout/PDF checks, Electron printToPDF, actual Windows printer testing |
| BN-01 | Bengali names/clinic/dentist/address/notes/instructions mixed with English saved/reloaded/previewed/PDF/physically printed with proper glyph shaping. Bundled font, no CDN. | Unicode integration/UI/PDF, fluent-reader physical print review |
| INV-01 | Unique transactional numbering, issued immutable items/totals, valid discount/tax/rounding; due = issued total−net valid allocations; status unpaid/partial/paid; draft edit vs controlled void; histories remain. | Integer/property/db constraints, invoices integration/E2E |
| PAY-01 | Amount/date/method/patient/invoice/receiver/reference/notes; configurable Cash/Bank/Card/bKash/Nagad/Rocket/Upay/Other; partial+second payment reaches exact zero; overpay rejected explicitly; reversal reason/actor/time preserves original. | Unit money/overpay, serialized payments/reversal ledger integration, UI |
| PAY-02 | Today-default collections by method/refunds/outstanding and all date filters; report reconciliation; no financial permission bypass. | Aggregate integration/multi-role/UI |
| STO-01 | All SKU/supplier/source/unit/prices/minimum/expiry/batch/location/notes; stock-in/out/adjust/waste/expired/damaged, purchases/current stock and history; quantities deterministic/nonnegative. | Ledger unit/integration/E2E |
| STO-02 | Low/out/expired/soon-expired notifications real and permission-aware; expired consumption prevention/configured disposition; inventory purchases accounting-linked atomically. | Controlled clock/stock integration, notification/UI |
| ACC-01 | Configurable expense/income categories including clinic costs, amount/date/method/vendor/reference/notes/attachment; corrections through ledger; no cash double-count. | Actual accounting integration/reconciliation/UI |
| ACC-02 | Daily revenue/collection/expenses/income/net cash/receivables/treatment/dentist/method/inventory/category/date summary; ranges show exact starts/ends; unauthorized reports/CSV/PDF rejected. | Integration expected aggregates, date edges, CSV formula injection, UI |
| STAFF-01 | Staff full requested fields/photo/status; salary/identification private; user username/hash/staff/role/status/last login; create/deactivate/reset/revoke audited. | Projection/escalation/last-admin integration, UI role permutations |
| FILE-01 | PDF/images/common documents/scans/prior Rx upload/metadata/preview/open/export/delete with permission, generated local path, byte validation not extension, size/disk collision handling. | Traversal/malicious filename/corrupt/ZIP doc integration + UI |
| LOGO-01 | PNG/JPEG/transparent/large/tiny/wrong/corrupt media accepted/rejected correctly; no distortion/overflow/alpha halo, local print render. | Upload decoder validation/UI/print visual review |
| SEARCH-01 | Global permitted patient/code/phone/appointments/visits/Rx/invoices/payments/inventory/staff/referral search with filters, debounce/pagination; unauthorized values/snippets absent. | Adversarial integration + large-data latency/UI |
| NOTE-01 | Real local upcoming/overdue/no-show/stock/expiry/outstanding/backup/restore/security events with links/read state and permissions; no online push/fake count. | Controlled clock/data/service/UI |
| BACK-01 | Native folder picker, timestamped all-data archive, consistent SQLite backup, manifest/hash validation, progress and truthful failure; 7/15/30 day while-running/startup due schedule and retention. | Engine backup integration/restart/disk failure/large memory test + native Windows dialog |
| REST-01 | Native backup file(s) picker; summarize/validate compatible chosen snapshot; corrupt/unsafe/newer backup rejected before touching current data; reauth+typed confirm; verified pre-restore backup always unless truly empty; safe replace/reopen/session revocation. | All-entity roundtrip, malicious archive, injected swap failure, packaged restore/restart |
| DEST-01 | Patient/record/user/reset/business removal permission+explicit confirm; high-risk password/phrase/final confirm and backup; ordinary uninstall preserves clinical data. | Adversarial integration/UI/install manual |
| SET-01 | All clinic/dentist/clinical/billing/inventory/print/security/backup/appearance/data settings validate/save/restart; per-user preferences distinct; safe defaults. | Typed settings integration/UI every tab |
| ABOUT-01 | Exact product/creator/email; actual version/build/SHA/schema/notices/support if configured; no invented biography. | UI/packaged metadata/license audit |
| AUD-01 | All specified sensitive actions actor/time/entity/success/failure tracked atomically, no password/code/token/PHI payload; controlled correction and immutable ordinary audit. Logs bounded, errors friendly and recoverable. | Service integration, secret/log scan, rotation/crash/UI |
| UX-01 | Every screen/interactive control all states, a11y/focus/labels/tab order, no clipping/overlap/dead action, restrained motion, density/theme/high-DPI/shortcuts/dirty forms. | Axe + exhaustive manual screen/control checklist at defined sizes |
| PERF-01 | Measure startup/dashboard/search/profile/timeline/invoice/Rx/DB/backup/restore, CPU/RAM; thousands/tens-thousands records, long documents/large attachments; rapid actions no crash/freeze. | Seeded test-only datasets/query plans/benchmark report |
| REC-01 | Forced close/interruption of writes/backup leaves recoverable DB/no half business records, reopens; disk/storage failures explained; no unhandled promise/error. | Kill/reopen integration + packaged soak |
| OFF-01 | Disconnect network: every listed core workflow works, no remote assets/API/telemetry; no development/test data/servers shipped. | Network-deny E2E + actual disconnected packaged machine |
| DEP-01 | Full direct/transitive runtime dependency inventory versions/purpose/licenses/bundling/network/redistribution; no known critical security/licensing issue/unused packages. | Lockfile inventory/audit/notices/package inspection |
| CI-01 | PR/push install/lint/types/unit/integration/frontend/db/build/audit; Windows native packaging/artifact upload; deterministic metadata/checksums; no merge/publish automation bypass. | Actual Actions runs and artifact evidence |
| WIN-01 | Real offline installer clean Windows/no dev tools: install/shortcut/icon/uninstall entry/launch/activate/setup/login/DB/print/PDF/backup/restore/files/restart/lock/update/uninstall with data preservation. | Actual clean Windows desktop evidence, not Linux assumptions |
| REL-01 | 15 gates passed, omissions pass 1/2/3, actual `.exe` plausible size/SHA/install/launch/workflow/uninstall, release notes/notices/reports/reproducible build, GitHub Release or actual validated dist artifact. | Signed-off release checklist + actual release URL/artifact, never fake |

## Critical E2E (complete chain)

Clean install → activate → clinic → two dentists/multiple credentials → staff/users/custom permissions → patient/profile → independent visit → adult/pediatric chart → C/C/O/E/diagnosis/treatment → multi-medicine Rx → preview/PDF/print → appointment/check-in/queue → invoice → partial+second payment/zero due → item/batch/consume → expense/accounting → native backup → restart → controlled restore → all-data verification → intentional unauthorized requests → auto-lock → audit → uninstall preserving data. Required on packaged Windows plus automated service/UI portions. Never mark the whole chain passed when only one portion ran.

## Security/negative cases

SQL metacharacters in all inputs, forged transport/user ID, direct hidden command, route/localStorage mutation, stale session, staff financial projection, account enumeration, brute force, unexpected fields, large/malformed JSON, Unicode control characters, filename traversal/encoded separators/device names, HTML/script injection in print, corrupt PDF/image, ZIP bombs/symlinks/duplicate entries/hash mismatch/new schema, arbitrary native path, unauthorized reversal/restore/reset, log credential leakage. Expected safe rejection, transaction rollback and no PHI/stack trace.

## Requirement coverage index (all master sections)

| Master sections | Governing artifacts/cases |
|---|---|
| 0–8 | IMPLEMENTATION_PLAN, ARCHITECTURE, AGENT_STATE, CI-01/DEP-01/REL-01 |
| 9–10 | SETUP-01/02, AUTH-01, ARCHITECTURE |
| 11–16 | SHELL-01, UX-01, DESIGN_SYSTEM, SCREEN_INVENTORY |
| 17 | DASH-01/PAY-02/STO-02 |
| 18–26 | PAT-01/02/03, VIS-01, CHART-01, TRT-01, REF-01, DATABASE |
| 27–28 | APT-01/QUE-01 |
| 29–34 | RX-01, PRINT-01/02/03, BN-01, INV-01 |
| 35–40 | PAY-01/02, STO-01/02, ACC-01/02 |
| 41–48 | STAFF-01, RBAC-01, AUTH-02, AUD-01, SEARCH-01, NOTE-01 |
| 49–55 | FILE-01, BACK-01, REST-01, DEST-01, SET-01, ABOUT-01 |
| 56–58 | UX-01, WIN-01, SCREEN_INVENTORY, DESIGN_SYSTEM |
| 59–66 | DATABASE, REC-01, PERF-01, RBAC-01, AUTH-01/AUD-01 |
| 67–74 | PRINT-01/02/03, BN-01, ACC-02, UX-01, FILE-01 |
| 75–84 | UX-01, BACK-01/REST-01, WIN-01, CI-01, REL-01 |
| 85–93 | All test layers/critical chain, PERF-01/RBAC-01, DEP-01, UX-01, LOGO-01 |
| 94–101 | INV-01/PAY-01/STO-01, APT-01/ACC-02, PAT-02, BACK-01/REST-01/REC-01 |
| 102–115 | OFF-01/NOTE-01/FILE-01/DEST-01/AUD-01, PRINT-01/02/03/BN-01, ABOUT-01 |
| 116–123 | All docs, feature-completion rule, no development records, REL-01 |
| 124–141 | RELEASE_CHECKLIST, 15 gates, multi-role/full manual QA, REL-01, AGENT_STATE |
| 142–150 | Plan invariants, critical chain, honest limitations, all acceptance cases, release evidence |

Final audit compares actual implementation to the original specification three times: per-feature mapping, user workflow verification, and independent omission/security/print/recovery pass. This coverage index does not substitute for those audits.
