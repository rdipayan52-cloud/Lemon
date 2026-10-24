# Lemon

**Lemon is an AI execution and automation layer for existing AI assistants.** It helps users turn requests in supported ChatGPT/Claude browser workflows into completed tasks through small offline agents, deterministic rule-based bots, and platform adapters.

## Product thesis

People already have an AI assistant they like. Lemon should let them keep using it, then bridge the gap between what they ask, what the model replies, and the actions that need to happen on the user's device or over the web.

## Intended request pipeline

1. Detect an explicitly supported ChatGPT or Claude browser session.
2. Read relevant user prompt and assistant response content from the permitted page context, with user-facing disclosure and an opt-out.
3. Use semantic retrieval and context extraction to identify the active request and relevant conversation state.
4. Send the minimum necessary task context to a configured decomposition model/API (for example, Groq-hosted models or Gemini, subject to provider terms, consent, and configuration).
5. Convert the decomposition into a validated typed task plan.
6. Dispatch suitable actions to small offline agents (including the intended Cactus/Spine agent approach) and deterministic rule-based bots.
7. Execute through approved browser/device/service adapters.
8. Verify results, persist task state, and report honestly.

The online model is a task planner/decomposer, not a trusted authority. Offline agents and bots should handle narrow, bounded skills. Deterministic validation and permission checks remain in control.

## Core promises and limits

- **Local-first execution where practical.**
- **No artificial per-action Lemon quota for local actions where feasible.**
- Not unlimited model inference, third-party access, or cloud resources.
- Do not assume browser access is unrestricted or that provider pages can be read without notice. Use supported browser extension APIs and comply with platform policies.
- Do not execute arbitrary model-generated shell code. Use typed, allowlisted actions.
- Ask before consequential actions, and make monitoring visible, pausable, and cancellable.

## Documentation

- [RFC 001: Vision, Problem & Product Philosophy](docs/rfcs/RFC-001-vision-and-problem.md)
- [RFC 002: System Architecture](docs/rfcs/RFC-002-system-architecture.md)
- [RFC 003: Task Execution Protocol](docs/rfcs/RFC-003-task-execution-protocol.md)
- [RFC 004: Offline Agents & Rule-Based Bots](docs/rfcs/RFC-004-offline-agents-and-bots.md)
- [RFC 005: Persistent Automation Engine](docs/rfcs/RFC-005-persistent-automation-engine.md)
- [RFC 006: Permissions & Security](docs/rfcs/RFC-006-permissions-and-security.md)
- [RFC 007: Assistant Browser Bridge & Context Capture](docs/rfcs/RFC-007-assistant-browser-bridge.md)
- [Architecture overview](docs/architecture/overview.md)
- [Task contract and implementation plan](docs/implementation/task-contract-and-plan.md)
- [ADR-001: Initial technical direction](docs/decisions/ADR-001-initial-technical-direction.md)

## Status

These documents define a proposed MVP contract and implementation plan, not a claim that the system has been implemented. Validate provider integration methods and the selected browser/OS targets before relying on them.
