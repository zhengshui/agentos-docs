# AgentOS Workspace Agent Guide

## Workspace Shape

This directory is an aggregation workspace for multiple AgentOS-related projects. It is not a single monorepo and the workspace root is not a Git repository. Most child directories are independent projects with their own dependency files, Git history, build commands, tests, and release cadence.

Use the root `README.md` as the product and directory map. When changing behavior, first identify the owning child project and work from that directory.

## Language And Documentation

- The root README and much of the project documentation are Chinese-first. Prefer Chinese for user-facing docs unless the target file is already English-only or bilingual.
- Keep project descriptions consistent with the Kit-oriented language in `README.md`: AgentKit, AppKit, LLMKit/ModelKit, ToolKit/OpenTool, Kernel, Auth/OAuth2, and ChatKit.
- If a new top-level project is added, update the root `README.md` directory map and then add detailed setup notes inside that project.

## Top-Level Project Map

- `agentos-api`: Dart/Shelf local AgentOS API gateway, default `http://127.0.0.1:8888`.
- `agentos-kernel`: Dart service-level kernel for memory, preference, RAG, Cron, health checks, and context.
- `agentos-server`: Java 21 Spring Boot server backend.
- `agentos-server-oauth2`: Java 21 Spring Boot OAuth2/OIDC authorization server.
- `agentos-web`: React/Vite/TypeScript/Ant Design web entry for the server edition.
- `agentos-dev-docs`: VitePress developer documentation site.
- `agentos-desktop`: Flutter desktop shell and local AI OS.
- `agentos_chat`: Flutter desktop chat client.
- `bret-browser`: Electron/React/TypeScript AgentOS-enabled browser.
- `wynn-wiki`: Tauri/Web personal knowledge base.
- `yoo-time`: Tauri/Rust/Web local-first work log and review tool.
- `code-switch`: Tauri/React tool for AI coding CLI and model configuration.
- `agentos-sdk-dart`, `agentos-sdk-ts`, `liteagent_sdk_dart`, `agentos-auth`, `chatkit-dart`, `chatkit-agentos-ts`, `cactus_openai`: SDKs and embeddable components.
- `opentool`, `opentool-dart`, `opentool-typescript`, `opentool-util-daemon`, `opentool-daemon-client`, `opentool-hub`: OpenTool protocol, SDKs, daemon, and registry-related projects.
- `agent-store-website`, `agentos-dcc-app`: application-store and business application services.

## Common Commands

Run commands from the relevant child directory.

### Dart / Flutter

```bash
dart pub get
dart test
dart analyze
flutter pub get
flutter test
flutter analyze
flutter run -d macos
```

For the local API gateway:

```bash
cd agentos-api
dart pub get
./tool/run_server.sh
```

For the desktop app:

```bash
cd agentos-desktop
git lfs pull
flutter pub get
flutter run -d macos
```

### React / Vite / TypeScript

Prefer the package manager indicated by the project lockfile.

```bash
npm install
npm run dev
npm run build
npm run typecheck
npm run lint
```

Projects using pnpm, such as `bret-browser`, `code-switch`, and parts of `agent-store-website`, should use pnpm scripts:

```bash
pnpm install
pnpm dev
pnpm build
pnpm test:ci
```

Useful known scripts:

- `agentos-web`: `npm run dev`, `npm run build`, `npm run typecheck`, `npm run lint`, `npm run openapi-ts`.
- `agentos-dev-docs`: `npm run docs:dev`, `npm run docs:build`, `npm run docs:preview`.
- `bret-browser`: `pnpm dev`, `pnpm build:web`, `pnpm test:electron`, `pnpm test:ci`.
- `code-switch`: `pnpm dev`, `pnpm build`, `pnpm typecheck`, `pnpm test:unit`, `pnpm format:check`.

### Java / Spring Boot

Use Maven from the relevant Java project root:

```bash
mvn clean install
mvn test
```

Java server projects often require local `.env` files copied from `.env.example` plus Redis, MongoDB, MariaDB, or OAuth2 configuration depending on the module.

### Rust / Tauri

For Tauri projects, use the project package scripts where present:

```bash
pnpm tauri dev
pnpm tauri build
cargo test
cargo check
```

## Development Rules

- Do not assume a root-level build or test command exists.
- Before editing, read the child project README and package/build files.
- Keep changes scoped to the owning project unless the request explicitly crosses SDK, docs, and app boundaries.
- Prefer existing SDK and Kit boundaries over direct cross-project coupling.
- For Server and Desktop integration, communicate through SDKs or Kit APIs where possible.
- For Embedded-related work, prefer reusing `agentos-api`, `agentos-kernel`, `opentool-util-daemon`, and `cactus_openai` instead of inventing another protocol layer.
- Avoid committing generated dependency folders such as `node_modules`.

## Verification Guidance

Pick the narrowest verification that proves the changed behavior:

- TypeScript UI or SDK changes: run the project typecheck/build, and lint or targeted tests when available.
- Dart or Flutter changes: run `dart analyze`/`flutter analyze` plus targeted tests.
- Java service changes: run module tests or `mvn test`/`mvn clean install` at the smallest relevant module root.
- Rust/Tauri changes: run `cargo check` or the project build/test script.
- Docs-only changes: run the docs build when touching VitePress config or generated navigation; otherwise proofread the affected pages.

If dependencies are missing, do not install globally. Use the child project's package manager and document any verification that could not be run.
