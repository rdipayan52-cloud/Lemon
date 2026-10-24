# ADR 001: Initial Technical Direction

- **Status:** Proposed
- **Date:** 2026-10-24

## Context

Lemon needs to validate a hybrid execution loop: browser context capture, remote task decomposition, local specialist agents/rule bots, approved platform actions, and verification. The team is small and should avoid premature distributed infrastructure.

## Decision

1. Use TypeScript for shared action contracts and validation where practical.
2. Use React for extension/companion UI if it suits the chosen platform.
3. Start with an explicit user-triggered bridge from supported ChatGPT/Claude browser pages.
4. Use one configured model API behind a provider-neutral decomposition interface.
5. Keep task planner, policy engine, orchestrator, handler router, and adapters separate.
6. Use strict typed action allowlists; do not run arbitrary model-generated shell commands.
7. Prefer local persistence, such as SQLite, for the local-first prototype.
8. Implement deterministic rule bots before adding many offline agents.
9. Evaluate the exact Cactus/Spine candidate for license, target hardware, memory, latency, and task accuracy before choosing it.
10. Add a hosted service only for a justified feature, such as remote operation while the device is offline.
11. Build one end-to-end vertical slice before expanding platforms.

## Consequences

- The first release supports fewer tasks but can be tested and audited.
- Assistant-page integration may break when provider UIs change and must fail safely.
- Model API calls remain subject to provider quotas, cost, terms, and data handling.
- Local automations may require the device to be online and awake.
- Automatic context capture needs clear disclosure, controls, and platform-policy review.

## Revisit when

- The team selects desktop browser versus Android as the primary platform.
- The selected offline agent/runtime is benchmarked.
- An explicit browser bridge proves insufficient for validated user needs.
- Local scheduling is insufficient for a supported use case.
