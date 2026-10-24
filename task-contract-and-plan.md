# Task Contract & Implementation Plan

This document converts the RFCs into an implementable first vertical slice. It is a proposal; adapt package names and platform choices after the team confirms the target.

## 1. First vertical slice

**Goal:** From a supported ChatGPT/Claude browser page, explicitly send one request to Lemon, decompose it with one configured model API, route a simple task to a rule bot or offline handler, execute through one adapter, verify it, and show a truthful result.

Do not start by supporting every app or arbitrary computer control.

## 2. Suggested repository layout

```text
lemon/
├── README.md
├── docs/
│   ├── rfcs/
│   ├── architecture/
│   ├── decisions/
│   └── implementation/
├── apps/
│   ├── browser-extension/       # capture, popup, task approval/status
│   └── local-companion/         # optional local execution service
├── packages/
│   ├── task-contract/           # schemas, types, validation
│   ├── planner/                 # provider interface and plan validation
│   ├── policy/                  # deterministic risk/permission decisions
│   ├── orchestrator/            # task/action lifecycle
│   ├── agent-router/            # routes to rules, offline agents, adapters
│   ├── rule-bots/               # deterministic handlers
│   ├── offline-agents/          # candidate local specialist integrations
│   ├── adapters/                # browser, files, reminders, notifications
│   ├── automation/              # durable scheduling and condition checks
│   └── storage/                 # task state, migrations, sanitized audit
├── tests/
│   ├── fixtures/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── .env.example
└── package.json
```

## 3. Initial action types

Start with a small allowlist:
- `research.extract_fields`
- `notification.create`
- `automation.create`
- `automation.pause`
- `automation.cancel`
- `reminder.create` only if a supported adapter exists
- `browser.navigate` and `browser.read_visible_content` within explicit domain and permission constraints

Do not add `shell.execute` or generic arbitrary code execution as an MVP action.

## 4. Provider abstraction

Define a common interface such as:

```ts
interface TaskDecomposer {
  decompose(input: NormalizedTask): Promise<ProposedPlan>;
}
```

Provider adapters implement this interface for the selected Groq-hosted model, Gemini, or another approved provider. Provider API keys belong in OS secure storage or environment configuration for development, never in source control. Provider output must be validated regardless of provider.

Choose one provider for the first implementation and create contract tests with recorded, sanitized fixtures. Add provider failover only after handling privacy, data residency, cost, and schema differences.

## 5. Development phases

### Phase 0: Decisions
- Pick the first platform and browser.
- Confirm the exact Cactus/Spine agent candidate, license, runtime, and hardware targets.
- Pick the first model API and configuration flow.
- Decide what context is captured and how the user can inspect it.

### Phase 1: Contracts
- Define Zod or equivalent schemas for task, action, plan, approval, result, and automation.
- Add task state transitions and reject invalid transitions.
- Write policy tests before connecting a real browser.

### Phase 2: Browser bridge
- Detect only supported assistant domains.
- Add explicit “Send to Lemon” interaction.
- Extract the latest relevant user prompt and associated response.
- Show a preview before sending content to a configured remote model.
- Add global/per-site disable controls.

### Phase 3: Decomposition and routing
- Integrate one LLM provider behind the `TaskDecomposer` interface.
- Validate all output against the action allowlist.
- Implement a rule bot for date/threshold/notification logic.
- Integrate one offline specialist agent only after evaluation.

### Phase 4: Execution and verification
- Add one supported platform adapter.
- Implement timeout, cancellation, retry classification, and result evidence.
- Verify postconditions and display success/failure/unknown accurately.

### Phase 5: Persistent automation
- Persist one scheduled task across restart.
- Implement pause/resume/cancel and a single condition monitor.
- Add polling budgets, deduplication, expiry, and failure handling.

### Phase 6: Hardening
- Prompt-injection tests.
- Permission revocation tests.
- Malicious and changed DOM fixtures.
- Secret-redaction tests.
- E2E tests for partial completion and ambiguous timeouts.
- Package size, memory, latency, and task accuracy measurements.

## 6. Definition of done for MVP

- A user explicitly starts a task from a supported assistant context.
- The user can inspect the context Lemon will send to the configured model.
- Model output is parsed and validated as typed actions.
- The policy gate blocks unsupported or unauthorized actions.
- A rule bot and an offline handler can be invoked through a common interface.
- One adapter executes a supported action and returns evidence.
- Task status survives a restart.
- A user can cancel the task.
- Failures, partial results, and unknown outcomes are reported honestly.
- Automated tests cover validation, permissions, retries, cancellation, and prompt injection.
