# Development and build instructions

At the Phase 0 checkpoint the repository contains planning only. Commands below will be replaced with verified tooling commands during Phase 1; there is no buildable installer yet.

## Environment

Node 22.22.3+ in the Node 22 supported line, npm with lockfile installation, Python/C++ toolchain when SQLite native prebuilt binaries are unavailable. Windows x64 packaging uses a Windows Actions runner/native dependencies. No paid service required. Dependencies are build-time network downloads, never runtime cloud dependencies.

Development preview uses a same-origin Vite proxy to an explicitly excluded local service adapter. Bind exposed preview to `0.0.0.0`, permit the Arena preview host, never send browser requests to `localhost` for another service. Core business logic is the same service as desktop IPC. Native Windows file/print operations cannot be represented as successful browser operations.

## Build policy

Use exact locked dependencies, production asset bundle and compiled worker/main/preload, no dev service/fixtures/sample DB. The static UI build is not a `.exe`. Do not invoke production installer packaging until pre-build gates pass; NSIS configuration/workflow may be prepared earlier. Native addon must be rebuilt for the selected Electron ABI on Windows. Capture exact version/SHA/date/schema/dependency state.

## Contribution

Work only on `arena/01a0f22f-dentiva-pro`, logical commits, no destructive force push, no main commits, no PR merges. Run affected and dependent tests; never disable failing tests. Update AGENT_STATE and TEST_EVIDENCE before pause. Do not place original activation code or real clinical information anywhere in source/fixtures/logs.
