# Release procedure

Target: one final production 1.0.0. No release exists yet. Follow RELEASE_CHECKLIST, never assume checkmarks.

1. Read AGENT_STATE, master acceptance mapping and exact candidate Git SHA. Ensure no real/test clinic records or secrets packaged.
2. Run actual lint/types/unit/integration/database/UI/E2E/offline/security/performance/dependency/license/code/UI/print/recovery audits. Record results, fix root causes, rerun regressions.
3. Complete gates 1–11 and clean Windows rehearsal. Building a UI or defining a workflow is not installer validation.
4. On Windows, rebuild native database addon for Electron, produce offline NSIS installer with owned ICO/notices/version/schema/SHA/build metadata. Do not invoke final packaging before required gates.
5. Verify actual `.exe` exists, plausible size and SHA-256. Test that exact installer on clean disconnected Windows with no development tools; full packaged clinic workflow/print/PDF/recovery/restart/permissions/lock/audit/update/uninstall.
6. Complete gates 12–15 and independent omission passes. Signing status documented; don't invent a certificate or hide unsigned Windows warnings.
7. Open/update review PR on fixed session branch with actual check/evidence links. **No agent merge.** Await explicit user merge instruction where appropriate.
8. Create final GitHub Release only after acceptance, attach actual installer/checksum/notices/release notes/test report. If a real platform restriction prevents Release upload, preserve actual validated `.exe` in `dist/` with provenance; no fake fallback.

No online auto-update. Per-user local clinical data is preserved on ordinary uninstall. Release notes include Windows support, installation/setup/activation without printing the secret, backup warning and Windows-exposed printer notes. Known serious failures block release, not a disclaimer.
