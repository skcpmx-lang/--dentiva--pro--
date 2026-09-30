# Production release checklist — 1.0.0

**Release status: NOT READY.** All gates initially pending. Do not build the production installer before required pre-build audits pass. A development UI bundle is not the production Windows `.exe`. No release claims/artifacts fabricated.

| Gate | Required evidence | Status |
|---|---|---|
| 1 Architecture | Offline trust boundaries, database/worker/IPC/native paths reviewed; production network disabled; risks documented | Pending |
| 2 Completeness | Master 0–150 mapped; every screen/action/tab real; all 15 feature criteria; omissions pass 1/2/3 | Pending |
| 3 Database | Migration/constraint/FK/transaction checks, crash integrity, ledger invariants/no orphans | Pending |
| 4 Security | Actual auth/RBAC attacks, no financial leak, activation/password/file/restore safety, safe logs | Pending |
| 5 UX/UI | Every screen/control/state/resolution/DPI/role, focus/keyboard/axe/contrast, no clipping | Pending |
| 6 Print | A4/A5/mini/thermal/custom, long Bengali/mixed content, 20+ Rx/50+ invoice items; PDF + Windows printer | Pending |
| 7 Recovery | Verified all-data backup/restore/restart, pre-restore backup, corrupt/malicious/failure tests | Pending |
| 8 Performance | Large DB/files benchmark/CPU/RAM/rapid navigation/soak/crash results vs target | Pending |
| 9 Dependencies | Actual lock inventory/license redistribution/notices/security audit/unused removal | Pending |
| 10 Code quality | Lint/types + inspected TODO/FIXME/XXX/placeholder/mock/fake/lorem/coming soon/debug/console/secret hits; no unfinished production code | Pending |
| 11 Automated | Full unit/integration/db/UI/E2E/offline suite ran and passed on exact candidate SHA | Pending |
| 12 Clean install | Actual clean Windows/no dev tools offline install, first launch/setup/DB/dialogs; documented data-preserving upgrade/uninstall | Pending |
| 13 Production build | Native Windows build/NSIS succeeded; output exists/plausible size/hash/metadata/package contents | Pending |
| 14 Installer | Actual generated installer opens/installs/launches, no undeclared downloads, icon/shortcuts/uninstall correct | Pending |
| 15 Final smoke | Packaged critical clinic chain, Bengali PDF/print, backup/restore/restart/multi-role/lock/audit/uninstall | Pending |

Clean-install rehearsal before final production build uses an explicitly classified validation candidate only after gates 1–11 pass; gate 13 builds final candidate from approved SHA, gates 14–15 retest that exact artifact. Never call a rehearsal artifact production-ready.

## Release-blocking conditions

Any unfinished module, dead control, crash, activation/login/database/patient/profile/visit/chart/Rx/invoice/payment/inventory failure, permission bypass/financial leak, printing/PDF/Bengali corruption, data loss, unsafe files/restore/reset, installation/uninstallation failure, inaccessible critical control, broken responsive layout, missing acceptance evidence or critical advisory blocks release. Do not suppress tests or hide serious limitations.

## Final artifact record

- [ ] Product/version 1.0.0, build number, UTC date, Git SHA, platform/architecture, schema version, Electron/Node/Chromium versions, lockfile digest.
- [ ] Actual Windows `.exe`, file size and SHA-256; installer test evidence tied to digest.
- [ ] No dev server/test DB/sample patient/password/test credentials/developer paths/source maps containing sensitive data.
- [ ] Owned alpha-correct ICO checked in desktop/Start/taskbar/window/installer/uninstall UI.
- [ ] Third-party bundled notices and licenses; full dependency inventory/SBOM.
- [ ] Test/acceptance/performance/security reports and release notes, offline install/setup/printer/backup notes.
- [ ] Signing status truthful. If unsigned, warnings documented; any commercial distribution decision reviewed, never invented trust.
- [ ] Review PR open and checks passing; **never merge without explicit user instruction**.
- [ ] Publish final GitHub Release/attach validated `.exe` only after gates. If actual GitHub platform limitation, actual validated artifact in `dist/`, not empty/placeholder.
- [ ] No online updater/telemetry/paid runtime dependencies; release reproducible from repo.

## Windows test environment record

Record OS version/build, VM cleanliness/snapshot, account permissions, disconnected network, screen/DPI, printer model/Windows driver/device type, Bengali reviewer, each workflow result, installer hash, uninstall/data preservation and update checks. A CI compilation alone cannot fill this record.
