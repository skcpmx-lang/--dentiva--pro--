# Screen inventory and product navigation

Every screen below is required for final acceptance, not a claim of current implementation. Authorization controls both navigation and service requests. Unimplemented modules have no production navigation entries. Common screen contract: real loading/empty/error+retry/success feedback, focused title, keyboard access, readable reduced-motion state, resilient scrolling, form validation, unsaved-change protection and contextual next action.

## First run / access

| ID | Screen | Primary actions / states |
|---|---|---|
| ACCESS-01 | Activation | Enter code; verified persisted receipt; invalid/backoff; no secret echo/log |
| ACCESS-02 | Setup clinic | Identity/contact/logo, hours/closed days, BDT, footer; validate/upload preview |
| ACCESS-03 | Setup dentists | Multiple profiles, multiple credentials/designations/certifications, registration, availability, signature/photo |
| ACCESS-04 | Setup administrator/review | Username/password confirmation, review all entries, atomic create, retry without duplicate clinic |
| ACCESS-05 | Login | Credentials, generic failure/throttle, successful entry, inactive user denial |
| ACCESS-06 | Lock | Reauthentication, preserved unsaved form, server-side denial behind overlay |

## Shared shell

Header: brand, configured clinic, Dhaka date, user, real notification count, quick actions, lock and profile menu. Sidebar groups and exact target items match the master specification; collapsed tooltips and keyboard focus; remember preference. Global search (Ctrl+K), notification drawer, account/password dialog, confirmation/reauth dialogs, printer preview/profile picker, diagnostic recovery boundary are shared.

## Practice

| ID | Screen | Required content / actions |
|---|---|---|
| DASH-01 | Dashboard | Explicit Today/7/30/90/year/custom range; real operational KPIs/appointments/queue/recent activity; financial KPIs/collections/outstanding only permitted; real low-stock/expiry alerts; authorized quick actions |
| PAT-01 | Patient list | Debounced advanced search, recent-first, date/status/dentist/gender/age/balance/last/next-visit filters, sortable columns, pagination, duplicate detection, register |
| PAT-02 | Register/edit patient | All identity/contact/medical/allergy/referral/tags fields; unique code; likely duplicate review and acknowledgement; save/cancel |
| PAT-03 | Profile identity + overview | Code/name/age/gender/phone/status/dentist; authorized balance/next appointment; safety allergies; patient-centric new visit/appointment/Rx/invoice/payment/referral/attachment |
| PAT-04 | Clinical Timeline | Paged chronological every-event history, timestamp/type/actor/details/link; permission projections |
| PAT-05 | Visits | List/detail; New Visit independent C/C, history, O/E, diagnosis, procedure/treatment, teeth, Rx/advice/followup/referral, invoice/payment/attachments; atomic bundles |
| PAT-06 | Dental Chart | Identified FDI adult/pediatric arches, orientation, keyboard/multi-tooth selection; configurable conditions; append/history/current states |
| PAT-07 | Treatments | Catalog-derived patient treatment history, status, tooth, dentist, dates, authorized cost |
| PAT-08 | Prescriptions | Patient list → view/editor/preview; dentist required; append longitudinal history |
| PAT-09 | Appointments | Past/upcoming patient schedule; booking/status/detail links |
| PAT-10 | Invoices | Authorized list/detail/issue/void/print and dues |
| PAT-11 | Payments | Authorized receipt/allocation/reversal history |
| PAT-12 | Financial History | Authorized reconcilable chronology of invoice/payment/reversal/adjustment |
| PAT-13 | Referrals | Add/edit/status/outcome, referred by/to, institution/reason/summary/followup |
| PAT-14 | Attachments | Upload validated bytes, metadata/date/actor, preview/open/export/delete-confirmation |
| PAT-15 | Notes | Patient-linked dated notes, authors, amendment history, permissions |
| PAT-16 | Audit/Activity | Permitted entity audit without credentials or unauthorized financial metadata |
| APT-01 | Appointment calendar/list | Day/week/month/list/custom, dentist/availability/status, filters, conflict validation, detail/editor, reminder |
| QUE-01 | Queue board/list | Check-in/number/priority/wait/dentist/status; call next/start/complete/cancel; no internet |

## Clinical

| ID | Screen | Required content / actions |
|---|---|---|
| TRT-01 | Treatment catalog | Name/code/category/description/default fee/duration/active/notes, per-dentist price, CRUD/archive |
| RX-01 | Prescription list | Patient/dentist/date filters/search/pagination; authorized create/open/preview |
| RX-02 | Prescription editor | Patient/visit/dentist, configurable C/C/O/E/advice, multiple medicines with form/strength/quantity/dose/timing/meal/duration/PRN/instructions, save/cancel |
| RX-03 | Prescription preview | Actual A4/A5/mini/thermal/custom output, printer/profile/margins, local PDF/print/cancel, handwriting area |

## Billing

| ID | Screen | Required content / actions |
|---|---|---|
| INV-01 | Invoice list | Date/patient/status/dentist/amount filters, search/page, permitted issue/view |
| INV-02 | Invoice draft/detail | Unique numbering, immutable issuance, item quantities/prices/discounts/tax, integer totals, allocations/status, reasoned void/reversal |
| INV-03 | Invoice preview | A4/A5/narrow/custom, local PDF/Windows print, long items/BDT/Bengali, no signature/Rx footer |
| PAY-01 | Payment dashboard/list | Today default; all date presets/range, real method totals/outstanding/refunds; allocations/receipts/reversal reasons |
| PAY-02 | Receive/reverse payment | Patient/invoice/amount/method/date/actor/reference/notes; outstanding constraint; controlled correction |
| STO-01 | Inventory items | Search/filter/alerts/current stock; SKU/category/supplier/location/unit/prices/minimum; add/edit/archive |
| STO-02 | Batches/movements | Lot/expiry/purchase source, stock in/out/adjust/waste/expired/damage, history; no negative inventory |
| STO-03 | Suppliers/purchases | Supplier profiles, purchase/movement/accounting links, histories |
| ACC-01 | Income/expenses | Authorized categorized entries, date/method/vendor/reference/notes/attachment; controlled reversals |
| ACC-02 | Reports | Explicit range; revenue/collection/expense/income/net cash/outstanding/treatment/dentist/method/category/purchases; CSV/local PDF |

## Administration

| ID | Screen | Required content / actions |
|---|---|---|
| USR-01 | Staff | Personal/photo/DOB/gender/address/phone/blood group/identification/department/joined/salary/emergency/notes/status; sensitive fields protected |
| USR-02 | Users | Username/linked staff/role/active/last login; create, deactivate, secure reset, revoke sessions; prevent removing last active administrator |
| USR-03 | Roles and permissions | Granular keys and preset explanation; custom roles; service enforcement; protect last admin/security privileges |
| BAK-01 | Backup | Native folder selection, manual/7/15/30 day settings/retention, progress/result/history/checksum/failure, secure-storage warning |
| BAK-02 | Restore | Native file(s) picker, manifest summary/compatibility/integrity report, explicit replace explanation, reauth/typed confirm, mandatory pre-backup, progress/restart/re-login |
| SET-01 | Clinic | Identity/logo/contacts/hours/closed days/footer/currency |
| SET-02 | Dentists | Independent profiles + multi credentials/signature/availability/prices |
| SET-03 | Clinical | Configurable C/C/O/E/advice/conditions/treatment catalog/Rx defaults |
| SET-04 | Billing | Invoice sequence/prefix, BDT, payment methods, discount/tax policy |
| SET-05 | Inventory | Categories/units/expiry/minimum thresholds |
| SET-06 | Printing | Templates/local fonts/paper/margins; separate Rx/invoice printer profiles |
| SET-07 | Security | 5/10/15/30 minute or admin-approved disabled lock; password policy/sessions/roles |
| SET-08 | Backup | Native location/schedule/retention/retry status |
| SET-09 | Appearance | Theme/density/sidebar/accessibility/reduced motion/text scaling |
| SET-10 | Data | Authorized exports; only safe documented imports if implemented; guarded reset/business delete with backup/password/phrase/final confirmation |
| AUD-01 | Audit log | Paged user/action/entity/date/success/failure search; metadata redaction; immutable ordinary permissions |
| ABT-01 | About/licenses | Dentiva Pro, Shohan Khan, helloiamshohan@gmail.com, version/build/SHA/schema/license/support if configured, no invented biography |

## Keyboard and layout specification

Ctrl+K search; Ctrl+Shift+P new patient; Ctrl+Shift+A appointment; Ctrl+Shift+V patient visit; Ctrl+Shift+R prescription; Ctrl+Shift+I invoice; Ctrl+S save current form; Ctrl+P dedicated document preview only; Escape close/cancel with dirty check. Never override OS shutdown or common copy/paste. Shortcuts permission aware and documented; don't fire typing-sensitive navigation while editing inputs.

Balanced six-KPI grid: 3×2 (standard), 6×1 (wide), 2×3 (compact); avoid accidental 4+2. Header/action wraps explicitly; table overflow is contained; modals max viewport height with internal scrolling. All button/icon states and ARIA relationships in DESIGN_SYSTEM. Final per-screen test sheet records each ID, resolution, role, action, status and issue.
