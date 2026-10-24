# Lemon Architecture Overview

## End-to-end loop

1. **Capture:** a supported browser bridge observes relevant ChatGPT/Claude conversation context under explicit controls.
2. **Retrieve:** semantic search fetches only relevant, bounded context and preserves provenance.
3. **Normalize:** identify the user's active goal, constraints, and missing information.
4. **Decompose:** a configured model API (for example, Groq-hosted models or Gemini) proposes a structured plan.
5. **Validate:** strict schemas and an action allowlist reject unsupported or malformed plans.
6. **Authorize:** deterministic policy checks permissions and required approvals.
7. **Route:** deterministic tasks go to rule bots; bounded language/perception tasks may go to offline specialist agents; actual side effects go through platform adapters.
8. **Verify:** check postconditions and retain evidence references.
9. **Report:** show success, failure, partial completion, or uncertainty.
10. **Persist:** the automation engine schedules future observations through the same guarded lifecycle.

## Core rule

**The online model decomposes. Offline agents handle narrow skills. Rule bots handle deterministic logic. Adapters perform side effects. Policy controls authorization. Verification decides what can be claimed.**

## Trust boundaries

Assistant responses, webpage content, emails, and documents are untrusted inputs. They may provide context but cannot expand permissions or override the user's intent. A model plan must never execute without schema validation and policy evaluation.

## Local-first and hosted operation

A local companion can manage UI automation, local files, task state, and schedules while the device is available. Optional hosted components may support remote tasks or notifications when the device is offline, but introduce cost, privacy, and operational complexity.

Make clear where each task runs and what must remain available. Do not promise remote persistence for a local-only task while its device is offline.

## Integration modes

- Explicit user-triggered browser bridge.
- Browser extension with narrow host permissions.
- Local companion for supported device actions.
- Direct service APIs where authorized.
- Mobile integration using supported OS mechanisms.

No single integration mode guarantees access to every application or provider.
