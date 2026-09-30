# Troubleshooting and operational safety

This document is established before implementation; actual messages/paths will be verified as the product is built.

- **Invalid login:** verify username/caps lock. Repeated failures have a bounded delay. Ask an authorized administrator for local password reset; no hidden master password.
- **Locked application:** reauthenticate. Do not discard unsaved notes or terminate the process to bypass lock. Service blocks protected reads/writes while locked.
- **Unavailable backup drive:** reconnect the authorized drive/select a new native folder as administrator, check space/access, retry. A failed backup must never be presented as complete.
- **Rejected restore:** preserve original data/backup. Compatibility, checksum, integrity or unsafe-entry errors require a valid compatible snapshot. Never manually overwrite an open SQLite database or its WAL.
- **Printer missing:** install/connect Windows printer driver first, verify Windows test page, select the appropriate profile/paper. Browser development preview does not exercise Windows native printing. Save a local PDF for inspection.
- **Bengali rendering:** product uses bundled shaping fonts. Do not substitute a font without glyph/shaping review. Report sample text without exposing real patient identifiers.
- **Unexpected shutdown:** reopen and allow integrity/recovery checks. Do not delete database/WAL files. Safe diagnostics contain no clinical request body or credentials.
- **Uninstall:** ordinary uninstall preserves clinical data under the Windows user account. Back up before upgrade/uninstall; a protected in-app reset is a separate destructive workflow.
- **Privacy:** anyone with OS administrator/file access may access local database/backups. Secure Windows accounts, device storage and external backups; application roles do not replace disk encryption.

Support identity: Shohan Khan — helloiamshohan@gmail.com. Do not send passwords, activation code, complete patient databases or sensitive records in an unsolicited support message.
