# Dentiva Pro

Offline Windows dental practice management for clinics in Bangladesh.

**Creator:** Shohan Khan · [helloiamshohan@gmail.com](mailto:helloiamshohan@gmail.com)

## Project status

Target: one production release, **1.0.0**. The product is **not released or production-ready**. Phase 0 repository discovery and complete engineering/product planning are documented; verified implementation/test status is in [AGENT_STATE](docs/AGENT_STATE.md). No installer has been validated. Never treat a development preview as a production release.

## Selected architecture

- Electron desktop runtime with an isolated React/TypeScript local UI.
- Worker-owned SQLite database; service-enforced granular permissions and transactional records.
- Entirely local clinical/financial workflows, English UI with Bengali Unicode content, BDT integer-poisha accounting.
- Dedicated adaptive prescription/invoice rendering, bundled Bengali fonts and local Windows printing/PDF.
- Verified all-data backup and safeguarded restore; offline NSIS installer, data-preserving uninstall.
- No required cloud account, paid API, telemetry, online updater or recurring runtime subscription.

## Engineering documents

- [Implementation plan and decisions](docs/IMPLEMENTATION_PLAN.md)
- [Architecture and trust boundaries](docs/ARCHITECTURE.md)
- [Database and ER design](docs/DATABASE.md)
- [Granular permission matrix](docs/RBAC.md)
- [Screen inventory](docs/SCREEN_INVENTORY.md) / [Design system](docs/DESIGN_SYSTEM.md)
- [Acceptance matrix](docs/ACCEPTANCE_TESTS.md) / [Testing strategy](docs/TEST_STRATEGY.md) / [Actual evidence](docs/TEST_EVIDENCE.md)
- [Backup and restore](docs/BACKUP_RESTORE.md) / [Printing](docs/PRINTING.md)
- [Development/build](docs/DEVELOPMENT.md) / [Release process](docs/RELEASE.md)
- [Release gates](docs/RELEASE_CHECKLIST.md) / [Third-party notices](docs/THIRD_PARTY_NOTICES.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md) / [Changelog](CHANGELOG.md)

## Safety and release policy

Do not store real patient information in development/test databases. Clinical data/backups are sensitive: protect the Windows account and storage with appropriate access control/disk encryption. Application RBAC is not protection against a local OS administrator. Fixed-code offline activation discourages casual exposure, not determined reverse engineering. Printers must be exposed by a supported Windows driver; no universal printer guarantee.

The fifteen release gates must pass before a final artifact is published. Actual clean-Windows installation, physical printing, packaged workflows and uninstall evidence are mandatory. Work stays on the session branch; pull requests are never merged without the user's explicit instruction.
