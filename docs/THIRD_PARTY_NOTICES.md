# Third-party dependency and license notices

Status: planning inventory only; no installed lockfile at Phase 0. Before release generate a full direct/transitive bundled inventory with exact installed versions, licenses and notice texts from the lockfile/package tree. This list is not a completed dependency audit.

## Selected dependency families

| Package | Expected license | Purpose | Bundled at runtime? | Runtime network? | Redistribution review |
|---|---|---|---|---|---|
| electron | MIT plus Chromium/Node notices | Windows runtime, sandbox, printers/PDF/DPAPI | Yes | No required; deny remote | Include Electron/Chromium/Node/LICENSES notices, native components |
| react / react-dom | MIT | Local UI | Yes | No | Preserve license/copyright |
| better-sqlite3 | MIT; SQLite public domain | Embedded database/backup | Yes/native | No | Include addon license, native Windows rebuild |
| zod | MIT | Untrusted payload validation | Yes | No | Include license |
| lucide-react | ISC | Consistent UI symbols | Yes, used icons only | No | Include copyright/license |
| yazl | MIT | Streaming ZIP backup output | Yes | No | Include license, audit transitive |
| yauzl | MIT | Strict lazy ZIP validation/input | Yes | No | Include license, reject unsafe entries |
| @fontsource/inter | OFL-1.1 | Bundled UI/print font | Font subset | No | Include OFL, reserved-name rules, no isolated font sale |
| @fontsource/noto-sans-bengali | OFL-1.1 | Offline Bengali shaping | Font subset | No | Include OFL/copyright; no CDN |
| typescript | Apache-2.0 | Compiler/types | No | Build only | Build dependency notices/inventory |
| vite / @vitejs/plugin-react | MIT | Bundling/dev preview | No | Development only | Exclude dev server from installer |
| tsx / esbuild | MIT | Dev/test TS + build | No | Build only | Audit native build distributions |
| vitest | MIT | Unit/integration | No | Test only | Not packaged |
| @playwright/test | Apache-2.0 | Real UI/desktop tests | No | Browser download during CI only | Not packaged; test browser notices separate |
| @axe-core/playwright | MPL-2.0 | Accessibility test checks | No | Test only | Test-only, not redistributed in product |
| electron-builder | MIT | Offline NSIS packaging | No (installer output includes applicable NSIS components) | Build tool downloads only | Include NSIS/external-tool notices as applicable |
| eslint / typescript-eslint | MIT | Static quality | No | Build only | Not packaged |

No dependency may introduce a paid API, cloud storage/auth, telemetry or commercial runtime subscription. Do not infer license safety from this expected list: inspect actual licenses, runtime transitive dependencies, security advisories and package contents after installation. Unknown/custom/copyleft runtime licenses require explicit review before packaging; removing a package solely to evade notice obligations is unacceptable. Fonts/logo have independent ownership/license records. Direct versions pinned only after toolchain validation; lockfile hash in release metadata.
