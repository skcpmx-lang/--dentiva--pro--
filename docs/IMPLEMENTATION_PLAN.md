# Dentiva Pro — master implementation plan

**Planning baseline:** 2026-09-30. **Target production version:** 1.0.0, unreleased. **Creator:** Shohan Khan. **Authority:** the user's master production specification (sections 0–150). This document does not relax that specification.

## 1. Phase 0 discovery

- Repository: `skcpmx-lang/--dentiva--pro--`, initially only a README; initial commit `4c7c3505d187c9db2bf129d4b790e1fc7f4451cd`.
- Session branch: `arena/01a0f22f-dentiva-pro`; all work stays on this branch. No other branch creation, switching, history rewriting, protection bypass or PR merge.
- Workspace: Debian 12 x64; Node 22.22.3, npm 10.9.8, Python 3.11, Git and gh available. C++/make and non-interactive sudo available. No installed Windows, Wine, NSIS, Chromium or desktop runtime at discovery.
- GitHub repository is public, not archived. Repository reading and branch push dry-run work. API permissions report no administrative capability; branch-protection and Actions-permissions APIs return HTTP 403. This does not establish whether `main` is protected. Checks and PR operations will be verified separately. Never assume permission or bypass protections.
- No existing product, database, design assets, workflows or customer data to preserve. Never put clinical development records into the release.
- Windows interactive installer/device tests cannot be performed on the initial Linux environment. A Windows CI runner can compile/test/package, but cannot by itself certify physical printers or a clean desktop installation. Those checks block release until evidenced.

## 2. Decisions made before production implementation

| Concern | Decision | Rationale / boundary |
|---|---|---|
| Desktop | Electron, bundled Chromium/Node; isolated React renderer | Mature Windows printer enumeration, `print`/`printToPDF`, predictable offline runtime, NSIS lifecycle; higher memory/disk than native UI is accepted and measured |
| Alternative evaluation | Tauri/WebView2 not selected; browser+service not selected; .NET host not selected | WebView runtime/bootstrap variability and print complexity; browser service exposure/startup lifecycle; a .NET host would duplicate web/print interop. Electron maintenance/security updates are necessary even without an online updater |
| Frontend | React, TypeScript, Vite, local fonts, authored CSS, Lucide icons | Typed reusable components; no dashboard template, remote CDN, chart service, online identity or telemetry |
| Local application layer | TypeScript business service in a worker thread; trusted Electron main broker | Database and slow hashing/backup work off the UI; no production HTTP port; all authorization enforced in the service |
| Transport | Narrow allowlisted IPC; same-origin HTTP adapter only for development/E2E | Same business logic under both; development server never packaged, arbitrary SQL/filesystem/shell access not exposed |
| Database | SQLite via `better-sqlite3`; one clinic per installation, multiple dentists | Foreign keys, WAL, FULL synchronous durability, migrations, online backup; one serialized business-write executor; no arbitrary record caps |
| Money | Integer poisha, bounded safe integers; integer quantities for invoice units | Deterministic rounding once per line/tax rule; credit/overpayment requires a deliberately implemented ledger rather than silent acceptance |
| Time | UTC instants; `Asia/Dhaka` clinical calendar; explicit local date boundaries | No reliance on workstation timezone for filters; date-only DOB/expiry distinct from timestamps |
| Passwords | Node cryptographic scrypt, unique 16+ byte salt, versioned work parameters, constant-time verification | Modern memory-hard password hashing without native cryptography addon; work factor benchmarked; no default user/password |
| Activation | Salted memory-hard verifier, constant-time digest check; installation-bound authenticated receipt protected with Windows DPAPI | Fixed-code offline activation deters casual extraction, not a local administrator/reverse engineer. Original code never enters repository, fixtures, output, documentation or release logs |
| Sessions/RBAC | In-memory session bound to trusted transport identity; server-side idle expiry, lock and brute-force throttling; granular permission keys | Neither routes, renderer state nor localStorage authorize requests. Role changes checked for each request; revoke on restore/deactivation/password change |
| Files | Per-user local application directory, generated content IDs, signature/content validation, no renderer-selected paths | Attachments never executable, no arbitrary paths or unsanitized HTML. DPAPI protects install key, not whole database; recommend Windows account access control/BitLocker |
| Backup | SQLite engine online backup, streamed ZIP with versioned manifest + SHA-256; staging then atomic rename | Includes DB, attachment objects, print/media assets stored as clinic objects; excludes sessions, installation activation key, logs and temporary files |
| Restore | Staged validation, integrity/FK/schema/hash checks, mandatory pre-restore backup, replacement not cross-clinic merging | Multiple files can be validated and one chosen. No ambiguous ledger/ID merging. Activation remains device-local; current sessions expire |
| Printing | Dedicated escaped templates + local Bengali fonts; isolated print window, native printer picker, local PDF | A4/A5 two-column clinical prescription; narrow/thermal stacked template, real page breaks, reserved handwriting area. Invoice has no signature/prescription footer |
| Packaging | electron-builder NSIS x64, per-user, fully offline, bundled runtime; data-preserving uninstall | Windows 11 supported target; Windows 10 22H2 compatibility assessed separately (OS vendor support has ended). No online auto-update or hidden downloads |
| Signing | Optional externally supplied publisher certificate only; no invented signer | Unsigned binaries trigger Windows warnings. Signing and clean-machine acceptance must be documented, never represented as tested if not |
| Testing | Vitest unit/integration against actual temporary SQLite; Playwright UI/E2E/a11y; Electron smoke; Windows lifecycle manual QA | Tests exercise real service and migrations, no mock production storage; fixtures never bundled |
| CI/CD | Linux check matrix + Windows native tests/build; package workflow behind acceptance evidence + manual dispatch | CI does not auto-merge or publish a production release; artifact checksum, SBOM/notices, metadata and gate report |
| Release | One final 1.0.0; reviewed PR, fifteen gates, actual installer verification, GitHub Release only after acceptance | No fake `.exe`, unvalidated release or placeholder `dist/`. User controls merge |

## 3. Product information architecture

1. First run: local activation → clinic + hours/contact/logo → dentists + multiple credentials/availability → administrator → setup review → dashboard.
2. Unauthenticated: login; locked session with retained unsaved form state; administrator-managed local credential reset. No unauthenticated patient access.
3. Practice: Dashboard; Patients (list → longitudinal profile); Appointments (day/week/month/list); Queue.
4. Clinical: Treatments/catalog; Prescriptions (list → editor → preview).
5. Billing: Invoice (list → editor → ledger/preview); Payments/collections; Inventory (items/batches/movements/suppliers); Accounting (income/expenses/reports).
6. Administration: Staff & Users (staff, users, roles/permission editor); Backup & Restore; Settings; About (version/schema/licenses/creator).
7. Cross-cutting: permission-aware global search, real actionable notifications, quick actions, profile menu, lock, keyboard shortcuts, diagnostics for administrators.
8. Patient profile tabs: Overview; Clinical Timeline; Visits; Dental Chart; Treatments; Prescriptions; Appointments; Invoices; Payments; Financial History; Referrals; Attachments; Notes; authorized Audit/Activity. Tabs only present when implemented and authorized during development; all requested tabs required for final acceptance.

## 4. Delivery sequence and dependencies

Every phase has an exit criterion. A compiled component is not a completed feature. Work proceeds in vertical slices with authorization, validation, persistence, UX states, tests, and documentation together.

| Phase | Deliverables | Exit evidence |
|---|---|---|
| 0 Discovery & planning | This plan, architecture, database/ER model, permissions, screens, design tokens/states, acceptance matrix, release checklist, execution state | All 20 pre-implementation decisions documented; no production code before this checkpoint |
| 1 Secure foundation | Repo/tooling/CI; migrations/connection; worker transport; activation; atomic onboarding; login/logout/lock; roles/audit; actual offline shell | Unit + actual SQLite integration + UI first-run/security tests; no leaked secret or sensitive DTO |
| 2 Patient-centric core | Patient validation/search/duplicates/filter/page; profile/timeline; independent visits; configurable findings; adult/pediatric historical chart; treatments/referrals/notes | Transaction rollback, pagination, archive safeguards, longitudinal history and permission tests; UI/a11y/small-window coverage |
| 3 Appointment flow | Availability/overlap rules/status transitions; calendars; check-in/queue/priority/call/start/complete; local reminders | Dhaka midnight/conflict/concurrent transition/E2E checks; real notifications |
| 4 Clinical documents | Multi-dentist prescription editor; multi-medicine structured timing; quick-select options; isolated preview/PDF/native printing | Bengali/20+ medicine/A4/A5/narrow/custom print tests; actual Windows printer/PDF verification |
| 5 Financial ledger | Catalog fees; invoice issuance/numbering; totals/discount/tax; partial allocations/reversals; payment-method configuration; financial projections/search/export | Integer arithmetic/property tests, immutable records, overpayment rejection, financial data boundary attacks, 50+ line printing |
| 6 Supplies & accounts | Supplier/item/batch ledger; stock-in/out/waste/expiry; purchase/expense transaction; income/expense reports, financial CSV/PDF | Negative stock prevention, batch expiry, reversal/audit/report reconciliation/inventory-accounting integration |
| 7 Administration & recovery | Complete staff/users/custom roles/settings; secure local media/attachments; manual + automatic verified backups; staged restore; protected export/reset | All-data backup/restore/restart roundtrip, malicious archives/files/path tests, pre-restore safety, session invalidation |
| 8 Product finish | Permission-aware dashboard/search/notifications; all screen states/shortcuts/themes/density/printer profiles | Every screen/primary control manually inspected, responsive/high-DPI/a11y audit, no unimplemented production routes |
| 9 Verification & hardening | Stress/crash/offline/multi-role; dependency/license/security/code/UI/print/database/recovery audits; omissions pass 1/2/3 | Evidence for gates 1–11; every requested capability traced, no release-blocking issue |
| 10 Windows release | Native build/NSIS; checksums/build metadata; clean installation, packaged QA, printer/uninstall tests; release notes/notices | Gates 12–15 passed on actual installer; review PR; no auto-merge; final GitHub Release or actual validated `dist/` artifact |

Core service APIs can be implemented ahead of corresponding screens but are not called complete until every feature-completion criterion is satisfied. Hidden/dead navigation is prohibited. No clinical records seeded into production: only role permissions, configurable clinical vocabulary, and payment methods are bootstrap reference data.

## 5. Implementation structure

```text
src/shared/       contracts, permission keys, safe money/date helpers
src/core/         migrations, SQLite repository, authentication, domain services
src/desktop/      main broker, sandboxed preload, worker host, native file/print adapters
src/renderer/     product screens, reusable accessible components, CSS design system
src/dev/          excluded development transport, temporary data directory
src/printing/     escaped adaptive document rendering
scripts/          audits, build metadata, gates, packaging helpers
tests/unit/       domain calculations/security
 tests/integration/ actual DB/workflow/rollback/recovery
 tests/e2e/       real browser + service workflows
 tests/desktop/   actual Electron and Windows checks
resources/        owned icon and bundled licensed local fonts
.github/workflows/ CI and gated packaging
 docs/            durable planning, acceptance evidence, operating instructions
```

Paths are finalized in tooling; no inaccessible functionality is published merely to match this tree. Temporary databases, test data, browser screenshots, artifacts and node_modules are ignored.

## 6. Business invariants

- Patient IDs are immutable local generated identifiers. Visible unique patient/visit/prescription/invoice/queue sequences are allocated transactionally, never by counting rows. Duplicate detection considers normalized name, phone, DOB/age; operator must deliberately acknowledge a likely match.
- Archive is normal patient/catalog/staff removal. Financial/clinical history is not cascade-deleted. Hard deletion is exceptional, authorized, authenticated, typed-confirmed, backed up and audited; impossible while legally relevant linked records exist unless protected full reset.
- Visits append longitudinal history. Signed/finalized clinical entries require reasoned amendments, not silent overwrites. Tooth events append; current chart is a derived latest view.
- Appointment status machine prevents completed/cancelled records returning to live queue unnoticed. Overlap/availability policy is server-enforced with an explicit authorized override reason when enabled.
- Invoice draft can change with permission. Issued totals/items immutable; void/credit/reversal workflows preserve the original ledger. Paid/due calculated from valid allocations; payment reversal records actor/reason/time. No negative invoices, negative payments, excessive discounts or implicit overpayments.
- Stock is a ledger per batch. Quantity cannot become negative through ordinary movement. Expired stock requires a permitted disposition. Purchase and associated accounting entry share a transaction.
- Dashboard/charts/reports reflect actual queries and explicit period boundaries. Financial aggregates are neither fetched nor serialized without their own financial permissions.
- Writes and audit events share a transaction where applicable. Failure does not leave half an invoice, visit, stock entry, role update, or setup.
- Backups claim success only after final artifact validation. Restore never changes current data before validation and verified pre-restore protection. Failure is actionable, not silently swallowed.

## 7. Nonfunctional targets (to measure, not claim)

- Startup target ≤5 seconds on supported SSD/8 GB Windows laptop; patient indexed search p95 ≤300 ms at 50k patients; dashboard p95 ≤1 second; first profile/timeline page p95 ≤500 ms.
- Benchmark 50k patients, 200k visits, 100k invoices/payments, 20k batch movements, and filesystem-scaled attachments. Dataset generation lives only in tests, outside packaged files.
- Page-size bounds are query protection, not record caps; counts and history retain all data. Bounded paginated timelines; debounce searches; short indexes checked with query plans.
- Crash/forced termination tests during multi-record commits/backup; SQLite integrity on reopen. Peak memory monitored for large backup (stream, not whole archive buffers).
- WCAG 2.2 AA intent with automated axe plus manual keyboard/focus/contrast, logical tab order and screen-reader labels. 960×600, 1280×720, 1920×1080, 2560×1440 and 4K/high-DPI inspected; narrow windows adapt, clinical tables intentionally scroll horizontally.
- No production network requests, remote fonts, analytics or online updater. Test offline by denying all network besides local application protocol. Local-administrator access is outside RBAC's security boundary and is documented honestly.

## 8. Definition of complete

Each feature requires all fifteen user criteria: working UI, database, business logic, validation, permissions, recoverable errors, loading/empty states, audit where applicable, automated tests, integration, responsive inspection, print/export where applicable and documentation. An unchecked acceptance case or Windows test is pending, not inferred from code.

## 9. GitHub/release discipline

Make logical commits on the fixed session branch, push only that branch, open/update a reviewable PR when integration permits. Never merge. CI on push/PR performs deterministic installation, lint/typecheck/unit/integration/UI/db/build/audit. Packaging needs separate gate evidence and Windows runs; production release manually approved after artifact validation. Capture SHA/build-date/runtime/schema/dependency lock state. No fake files, no fake statuses, no plaintext activation code in any repository content.

## 10. Continuation protocol

`docs/AGENT_STATE.md` is authoritative for unfinished steps. Before pause: record current phase/task, changed files, actual tests, pending tests, known issues/blockers, exact next command, CI/PR/build/release state. On “Continue”: inspect repo/status/branch/PR, read state/evidence and finish the interrupted task first. Never restart approved planning or claim unfinished modules are complete.
