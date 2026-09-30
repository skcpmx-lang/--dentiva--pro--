# Testing architecture

Tests are verification, not product data. Never bundle fixtures, databases or test credentials into production. Each suite uses fresh temporary paths, no connected customer data and no online API.

## Layers

- **Unit (Vitest):** exact-poisha calculations and rounding/overflow; payload validation/duplicates; permission registry/matrix/projections; FDI numbering/events; Dhaka boundaries/leap dates/range display; prescription escaping/formatting; manifest metadata; unrelated-secret activation verifier, hash/malformed hashes; throttle/idle timeout clock.
- **Database/integration (Vitest + actual better-sqlite3):** migrations/checksums/FKs/unique constraints, setup rollback, patient/visit/history/chart/Rx/invoice/payment atomic workflows, stock/accounting, queue/status/conflicts, all role grants/denials and stale sessions, append audit, backup/restore of all classes, corrupt archive/disk/worker failure. Deliberately inject errors inside critical transactions and verify zero partial rows.
- **Frontend/E2E (Playwright):** real dev transport/service and temp database, activation fixture verifier distinct from production, full onboarding/patient/clinical/ledger workflows, forms/dates/search/pages/dialogs/dirty checks/lock, custom roles/direct HTTP calls, keyboard/a11y/responsive/reduced motion. No fake API stubs standing in for the production data service.
- **Desktop (Playwright Electron/Windows):** real context-isolated preload, sender guard, no Node escape, local asset protocol, native dialogs, actual `printToPDF` with bundled fonts, start/restart/lock/crash. Windows CI native addon rebuild validation.
- **Manual:** every SCREEN_INVENTORY ID/control/tab/modal/save/cancel/filter, each role, all viewport/DPI/keyboard states, visual prescription/invoice/Bengali shaping/physical print, actual clean install/update/uninstall and packaged critical chain.

## Large dataset / stress

Test-only generator for 50k patients, 200k visits, 100k invoice/payment records, 20k inventory movements and attachment volume bounded by test host capacity. Measure p50/p95 and query plans, startup/profile/timeline/dashboard/document generation/backup/restore, peak memory, CPU, archive streaming. Rapid search/navigation, 20+ meds/50+ lines, repeated login/preview/restart, concurrency, forced interruption. No artificial record caps introduced to pass benchmarks.

## Offline and security

Deny remote requests with browser/network instrumentation; production package inspection for CDN/analytics/dev servers. Direct unauthorized commands bypassing UI, forged identity/localStorage/permissions, stale/expired sessions, login/code brute force, SQL strings, path traversal/device names/signatures/corrupt media/archive bombs/duplicate entries, markup escaping, unsigned backup tampering, destructive reauth bypass, secret/log scan. Negative tests expect safe English errors and intact storage.

## Tooling / evidence

All commands documented in README as implemented. Lockfile install and lint/types/db/unit/integration/UI/static build/audit in CI; Windows stages clearly separate. Collect reports/artifacts/checksums without PHI or credentials. TEST_EVIDENCE captures command/date/commit/platform/results and actual failures/remediation. AGENT_STATE identifies pending suites and exact next action. Failed tests must be fixed at root and rerun with dependents; never disabled or represented as passed.
