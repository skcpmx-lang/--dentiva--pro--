# Backup and restore design / operating contract

Implementation and test status: see AGENT_STATE and TEST_EVIDENCE. This contract is required for release.

## Contents and privacy

A timestamped `DentivaPro_Backup_YYYY-MM-DD_HH-mm-ss.dentiva-backup` is a streamed ZIP containing `manifest.json`, a consistent `clinic.sqlite`, and generated-ID object files for patient attachments, clinic logo, dentist/staff photos and configured signatures. All settings, roles/users (password **hashes** only), patients, histories, appointments, queue, prescriptions, invoices/allocations/reversals, stock/batches/ledger, expenses, notices and audit history are in the database. Manifest records format version, product/schema version, UTC creation instant, Dhaka display timestamp, file sizes and SHA-256 per file.

Excluded: installation activation receipt/DPAPI key, live sessions, app executable, diagnostics, temporary files, test records. Target machine needs activation independently. Backups contain sensitive clinical/financial information and password hashes; baseline backup is not encrypted. UI warns to secure folders/removable media/Windows account/disk. No cloud upload.

## Manual workflow

Permission `backup.create` → native Windows directory picker → private capability returned by main (no renderer path text) → show selected folder and contents warning → Create backup. Worker coordinates mutations, takes SQLite engine online backup, verifies DB snapshot/integrity and object references, streams object snapshot into a same-directory staging archive, hashes and validates manifest, fsync/closes output, then unique atomic rename. Collision never overwrites old backups. Success only after actual finished file verification; audit/backup record and local notification. Safe progress/errors, re-enable UI on failure and remove incomplete staging output. Never copy live `.sqlite` ignoring WAL.

## Automatic

Authorized configuration: native folder capability persisted internally; enabled interval 7/15/30 days; validated retention count/age. Check due on startup and periodically while app runs, only one job at a time. Missed period runs next available opportunity. Power-off/closed application does not produce backups; this limitation is stated. Unavailable drive/disk full denied access is a recorded actionable failure, not success. Retry schedule with bounded backoff, no busy loop. Retention deletes only this product's verified scheduled archives after a successful new backup; never arbitrary files or sole/pre-restore archive silently.

## Restore workflow and safeguards

1. `backup.restore`, current administrator reauthentication, native file(s) picker. Multiple files may be validated and summarized; operator chooses one complete snapshot. No unsafe automatic combining of unrelated SQLite ledgers.
2. In staging, validate format/product/schema compatibility, entry names, no traversal/absolute paths/symlinks/duplicates, declared vs actual size, SHA-256, compression expansion/disk space, allowed object IDs and all database object references. Open snapshot read-only for `integrity_check`, `foreign_key_check`, supported migrations and business constraints. No code execution from archive. Reject unsupported future schema before touching current data.
3. Show clinic/time/count summaries and exact replacement effect. Typed `RESTORE CLINIC DATA`, password and final confirmation. No one-click destructive restore or silent cross-clinic merge.
4. **Always create and verify a pre-restore backup** if any current business/config/user data exists. Empty means truly no data to protect, not merely zero patients. If pre-backup fails, abort restore. Prefer original selected backup directory or authorized safe local recovery directory on available volume; show location.
5. Enter maintenance mode, drain/deny writes, close DB and handles. Swap staged data/object directories on same volume with original retained for rollback. On failure restore original. Verify reopened integrity/references; remove obsolete WAL/SHM only as part of safely closed replacement.
6. Invalidate every session, preserve device activation, append local restore audit/provenance and notify. Require login to restored users; never reuse old admin identity for restored clinic blindly. Reopen UI with clear outcome. Retain pre-backup and rollback source until successful validation/cleanup policy.

## Verification

All-data roundtrip: clinic/two dentists/multi credentials/staff/users/roles → patient/medical/allergies/tags → repeated visits/charts/treatments/referrals/notes/Rx → appointments/queue → invoices/two partial payments/reversal → inventory/batches/purchase/consumption/waste → expense/attachment/settings/audit → backup → close/reopen → safe restore → verify every entity/count/value/content hash and ledger balance. Corrupt/truncated archive, wrong schema, malicious entries, huge archive, missing object, interrupted backup, disk full, permission loss and failure during swap all tested. Physical Windows native pickers and packaged reopen tested separately. No passing result without actual evidence.
