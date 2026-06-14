# AGENTS.md Generator Prompt

> Copy this prompt into any new project. Fill in the `[VARIABLES]` section at the top, then send to Claude. The design philosophy, architecture patterns, SOLID rules, TDD discipline, logging framework, and documentation standards remain fixed. Only the tech stack and project-specific details change.

---

## How to use

1. Fill in every `[VARIABLE]` below.
2. Paste the entire prompt into a new Claude conversation.
3. Claude will generate a complete `AGENTS.md` + `project-context.md` pair, ready to commit at your project root.

---

## The Prompt

````
You are generating two files for a new software project:
- `AGENTS.md` — the project guidelines and agent rules file
- `project-context.md` — the living project context kept in sync with AGENTS.md

Both files must be generated together and must cross-reference each other with a sync reminder at the top and bottom.

---

## [VARIABLES] — fill these in before sending

PROJECT_NAME: [e.g. "EV Charger Management System"]
PROJECT_DESCRIPTION: [1–2 sentences on what the system does]
PRIMARY_LANGUAGE: [e.g. TypeScript / Python / Go]
RUNTIME: [e.g. Node.js LTS / Python 3.12 / Go 1.22]
FRAMEWORK: [e.g. NestJS / Express / FastAPI / Gin]
ORM_OR_DB_LAYER: [e.g. Prisma / SQLAlchemy / GORM / raw pg]
PRIMARY_DATABASE: [e.g. PostgreSQL / MySQL / MongoDB]
SECONDARY_DATABASE: [e.g. DynamoDB / MongoDB / none]
CACHE: [e.g. Redis / Valkey / none]
MESSAGE_BROKER: [e.g. Kafka / RabbitMQ / MQTT / none]
LOGGING_TOOLS: [e.g. Pino + Chalk / Winston / Zap / structlog]
PROCESS_MANAGER: [e.g. PM2 / Docker / systemd]
CI_PLATFORM: [e.g. GitHub Actions / GitLab CI / Bitbucket Pipelines]
TEST_FRAMEWORK: [e.g. Jest + Supertest / pytest / Go test]
ADDITIONAL_TOOLS: [any other tools: e.g. BullMQ, WebSockets, Prisma Studio]
MODULES: [list the initial domain modules, e.g. Auth, User, Device, Session, Notification]

---

## Fixed Design Philosophy — DO NOT change these regardless of tech stack

### Architecture mindset
The codebase is built on three pillars:

1. **Trust boundaries** — every layer is only allowed to see the layer directly below it, and only through an interface or abstraction, never a concrete class. The layer stack is a firewall, not a preference.

2. **Explicit contracts** — nothing is implied; everything is written down. Intent lives in JSDoc/docstrings on every method (public, protected, and private), OpenAPI specs on every endpoint, and Mermaid diagrams on every module. Code tells you what runs; contracts tell you what was meant.

3. **Fail loudly, early** — if a required environment variable is missing, the module refuses to start. Tests gate code, not the other way around. Typed errors only — never throw raw strings. Silence and implicit success are treated as bugs.

The goal: a codebase that polices itself, not one that relies on developers remembering the rules.

---

### Strict layer stack (adapt naming to the framework, never skip a layer)

```
Global Utils / Libraries (package manager packages)
  └── Modules
        ├── ModuleContext       (env validation at startup — throws if anything is missing)
        ├── Repository          (database abstraction — exposed only via Interface)
        ├── Service             (business logic — receives Repository via Interface only)
        ├── Controller          (DTO validation — calls Service, returns nothing raw)
        └── Router / Middleware
```

Rules:
- Repository → Service boundary: Repository exposes an Interface. Service depends only on the interface, never the concrete class.
- Service → Controller boundary: Service returns DTOs. Controller consumes DTOs and constructs the API response.
- No layer may skip another. Controllers never call repositories directly.
- No cross-module repository calls. If module A needs data owned by module B, it calls module B's service.
- No business logic in controllers. No DB calls in services directly.

---

### ApiResponse contract

All responses — success and failure — go through a shared ApiResponse utility. No controller returns a raw object.

```
ApiResponseShape<T> {
  success: boolean
  data: T | null
  message: string
  version: string
  traceId: string        // correlation ID from request context
  deprecatedWarning?: string
}
```

---

### Module versioning

- Each module's controller maintains its own version string (v1, v2, etc.).
- The version is always included in every ApiResponse.
- Clients may request an older version by passing a `version` field in the JSON request body (e.g. `{ "version": "v1" }`). This is a backward-compatibility parameter in the payload, not a header.
- Deprecated versions return a deprecatedWarning field with a sunset date.
- Sunset versions return HTTP 410 Gone.

---

### ModuleContext pattern

Every module declares a ModuleContext (or equivalent) static/singleton object. It is populated once at module startup and throws immediately if a required environment variable is missing.

For NestJS projects, ModuleContext implements `OnModuleInit` so that `load()` is called automatically by the NestJS lifecycle before the module's services are used:

```typescript
// [module]/[module].context.ts
export class [ModuleName]ModuleContext implements OnModuleInit {
  static [ENV_VAR_NAME]: string;

  onModuleInit(): void {
    [ModuleName]ModuleContext.load();
  }

  static load(): void {
    const value = process.env.[ENV_VAR_NAME];
    if (!value) throw new Error('[[ModuleName]Module] Missing env: [ENV_VAR_NAME]');
    [ModuleName]ModuleContext.[ENV_VAR_NAME] = value;
  }
}
```

For non-NestJS frameworks, call `ModuleContext.load()` explicitly at app startup before any module services are instantiated.

Environment variables are never read with process.env (or os.environ, os.Getenv, etc.) outside of ModuleContext.load(). Never hardcode secrets, URLs, or environment-specific values.

---

### Coding standards (adapt syntax to PRIMARY_LANGUAGE)

Naming conventions:
- Constants: ALL_CAPS
- Classes: PascalCase
- Interfaces: PascalCase prefixed with I (or use the language's idiomatic interface naming)
- Types / Type aliases: PascalCase
- Methods / functions: camelCase (or snake_case if that is idiomatic for PRIMARY_LANGUAGE)
- Files: kebab-case
- Environment variable keys: ALL_CAPS

Type safety:
- No `any` (TypeScript) / no untyped `dict` without a schema (Python) / no interface{} without assertion (Go). Use the language's strongest strictness settings.
- Strict mode always on.
- All function return types must be explicitly annotated.

Async and error handling:
- All async functions must handle errors — no unhandled promise rejections or uncaught exceptions.
- Never throw or raise a plain string. Always raise a typed error subclass.
- Custom error classes are the only permitted use of class inheritance (if the language uses inheritance at all).

DTOs and validation:
- DTOs must use a validation decorator or schema library appropriate for PRIMARY_LANGUAGE (e.g. class-validator for TypeScript, Pydantic for Python, validator tags for Go).
- DTOs are the only objects that cross the Service → Controller boundary.
- Never pass raw DB model objects out of the repository layer.

General:
- All functions and class methods — public, protected, and private — must have a docstring / JSDoc comment.
- File length limit: 300 lines. Exceeding this is a signal to split responsibilities.
- One class per file. Utilities may export multiple functions from one file if tightly related.
- Composition over inheritance. No class inheritance except for custom error types.

---

### Docstring / JSDoc standard

Every function, method, and class must have a documentation block — no exceptions.

Required tags (adapt to language idiom):
- Summary / description (what it does, not how)
- @param / :param (one per parameter, name + meaning, no type repetition if types are in the signature)
- @returns / :returns (what is returned and when)
- @throws / :raises (error class + the condition that triggers it)
- @deprecated (migration path + removal version)
- @internal (private/protected — signals not part of public contract)
- @example (utility functions — at least one usage example)

Automated enforcement: configure the language's linting tool (eslint-plugin-jsdoc, pylint, golangci-lint, etc.) to require docstrings on all public and private methods. This runs in CI and blocks PRs that violate the standard.

---

### Utility decorators

Cross-cutting utilities (retry, caching, rate-limiting) must be implemented as decorators to keep business logic classes clean. The following two are standard — generate them in `lib/decorators/` for every project:

**Exponential retry** — for operations that may fail transiently (network calls, external API calls):
```
ExponentialRetry(maxRetries: number = 3, baseDelayMs: number = 100): MethodDecorator
```
- Retries up to `maxRetries` times with exponential backoff: `baseDelayMs × 2^(attempt - 1)`.
- On final failure, rethrows the original error.
- Applied as a method decorator on repository or service methods, never on controllers.

**In-memory map cache** — for short-lived, single-instance transient caching:
```
InMemoryCache(ttlMs: number = 5 * 60 * 1000): MethodDecorator
```
- Caches by a key derived from method name + serialised arguments.
- Evicts on TTL expiry (checked at read time, not eagerly).
- Only for single-instance, non-distributed caching. Use the project's CACHE layer (Redis/Valkey) for distributed caching.

Both decorators must be generated with full JSDoc, adapting syntax to PRIMARY_LANGUAGE. If the language doesn't support decorators natively, implement equivalent higher-order function wrappers following the same contracts.

---

### SOLID principles (enforced)

SRP — Every class has exactly one reason to change. Each layer owns only its concern.
OCP — Add new behaviour by adding new code (new strategy, new handler), not by editing existing stable code.
LSP — Any implementation of an interface must be substitutable without breaking callers. Mock implementations used in tests must honour the full interface contract.
ISP — No class is forced to implement methods it doesn't use. Split fat interfaces into focused ones.
DIP — High-level modules depend on abstractions, not concretions. Concrete classes are wired at the module composition root (startup), not inside business logic.

---

### Testing methodology

TDD strictly enforced:
1. Identify the feature / change.
2. Write a test case list in plain English (no code).
3. Submit for review → get approval.
4. Implement tests (red).
5. Implement code (green).
6. Refactor (still green).
7. Open PR only when all tests pass.

No implementation begins without approved test cases. Test approval is a gate, not a formality.

Test layers:
- Unit: service logic, repository logic in isolation — mocked dependencies
- Integration: module end-to-end within the app
- Contract: repository interface compliance (LSP verification)
- E2E: full HTTP request → DB round trip

Coverage thresholds (minimum):
- Branches: 80%
- Functions: 90%
- Lines: 90%
- Statements: 90%

---

### Logging framework

Tools: use LOGGING_TOOLS. Structured JSON in production, pretty/coloured in development.

Log levels:
- fatal: app cannot continue
- error: handled error, app continues
- warn: deprecated API called, retried operation, recoverable state
- info: normal significant events (startup, shutdown, migration)
- debug: detailed internals — dev/staging only
- trace: request-level detail — only when explicitly debugging

Correlation / trace ID:
- Every inbound HTTP request is assigned a traceId at the middleware layer before any handler runs.
- Read X-Request-ID header if present; otherwise generate a UUID v4.
- Store in AsyncLocalStorage (or equivalent context mechanism for PRIMARY_LANGUAGE).
- Attach to every log line automatically via the logger's mixin/hook.
- Return in every ApiResponse as the traceId field.

Structured log shape (every line):
```
{
  "level": "info",
  "time": "<ISO8601>",
  "traceId": "<uuid>",
  "module": "<ModuleName>",
  "method": "<HTTP method>",
  "path": "<route path>",
  "userId": "<actor id or null>",
  "durationMs": <number>,
  "msg": "<human readable message>"
}
```

Audit trail (separate log stream — never mixed with app logs):
- Tracked events: auth events, CRUD mutations, API access (every authenticated request), system events (startup, shutdown, migration, unhandled error).
- Audit log shape: auditEvent, traceId, actorId, actorRole, targetResource, targetId, outcome (SUCCESS | FAILURE), ipAddress, userAgent, timestamp, meta.
- Audit logs are append-only. Never update or delete an audit record.
- In production, ship audit logs to a durable external sink.
- Failed actions logged with outcome: FAILURE and errorCode in meta.
- System events use actorId: SYSTEM.

Never log passwords, tokens, API keys, PII, or raw request bodies that may contain credentials.

---

### Documentation framework

Location: {project root}/docs/{module}/

Required per module (all P0 — mandatory before a module PR can be merged):
- overview.md
- api.yaml (module-level OpenAPI spec)
- diagrams/dependency-flowchart.mmd
- diagrams/sequence-{primary-flow}.mmd
- diagrams/class-diagram.mmd
- diagrams/er-diagram.mmd (P1 if schema exists)
- diagrams/state-{entity}.mmd (P1 for stateful entities)

Mermaid-first rule: diagrams come before text explanations.

Every PR touching a public interface or behaviour must update the relevant diagram. Outdated diagrams are treated as bugs.

Architecture Decision Records (ADRs) are required for any decision that changes a cross-cutting concern (auth, logging, error handling, versioning, database layer).

OpenAPI is the source of truth for the public API. Every endpoint must document:
- summary (verb-first, one line)
- description (2–3 sentences)
- operationId (camelCase, unique)
- security ([] public, [BearerAuth] protected)
- all request parameters and body schemas
- all response status codes that apply (200/201/204/400/401/403/404/409/410/422/429/500)
- x-version (which controller version this doc covers)
- x-deprecated + x-sunset-date if applicable

Public docs must never mention: repository interfaces, service class names, ModuleContext, ORM internals, internal error types, or file paths.

---

### Phase gates (Waterfall delivery model within TDD)

| Phase         | Entry criteria         | Exit criteria                  |
|---------------|------------------------|-------------------------------|
| Requirements  | Stakeholder sign-off   | Approved spec doc              |
| Design        | Approved spec          | UML diagrams reviewed          |
| Implementation| Test cases approved    | Code + tests passing           |
| Testing       | Implementation complete| Coverage thresholds met        |
| Deployment    | All tests green        | Deployment checklist signed    |

---

## What to generate

Using all of the above, and the [VARIABLES] I filled in:

1. Generate `AGENTS.md` — the complete project guidelines and agent rules file. Adapt all code examples, naming, and tool references to PRIMARY_LANGUAGE, FRAMEWORK, and the listed MODULES. Include all sections: Project Lifecycle, Project Rules, Coding Standards, SOLID Principles (with Mermaid diagrams), Tech Stack table, Testing Methodology, Documentation Framework (with all Mermaid diagram templates adapted for the first listed module), Logging Framework.

2. Generate `project-context.md` — a living context document that summarises: project purpose, current modules and their status, key architectural decisions made so far, known tech debt, and open questions. Start it in an "initial setup" state.

3. Both files must start and end with a sync reminder that they must be kept in sync and committed together.

4. The database client pattern: generate a `database/client` equivalent appropriate for ORM_OR_DB_LAYER, with subfolders for SQL schema, NoSQL models (if SECONDARY_DATABASE is set), and cache definitions (if CACHE is set).

5. For MODULES: generate the full folder structure skeleton and the ModuleContext class for each module, with the correct environment variable names derived from the module's purpose.

Output both files in full, in order: AGENTS.md first, then project-context.md. Use markdown code blocks to separate them clearly.
````

---

## Quick reference — what stays fixed vs what changes

| Fixed (never changes) | Variable (fill in per project) |
|---|---|
| Three-pillar mindset (trust boundaries, explicit contracts, fail loudly) | Language + runtime |
| Layer stack (ModuleContext → Repo → Service → Controller → Router) | Framework |
| ApiResponse contract shape | ORM / DB layer |
| Repository interface pattern | Databases + cache |
| ModuleContext env validation | Message broker |
| Composition over inheritance | Logging tools |
| JSDoc / docstring standard + ESLint enforcement | CI platform |
| SOLID principles | Test framework |
| TDD gate workflow | Module names |
| Coverage thresholds (80/90/90/90) | Project description |
| Waterfall phase gates | Additional tools |
| Pino-style structured logging + audit trail shape | — |
| Trace ID / AsyncLocalStorage pattern | — |
| OpenAPI per module + merged shared spec | — |
| Mermaid-first documentation | — |
| ADR requirement for cross-cutting changes | — |
| 300-line file limit | — |
| No process.env outside ModuleContext | — |
| Append-only audit log | — |
| Utility decorators (ExponentialRetry, InMemoryCache) | — |
| Version negotiation via request body `{ "version": "v1" }` | — |
