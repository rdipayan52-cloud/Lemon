# RFC 007: Assistant Browser Bridge & Context Capture

- **Status:** Proposed
- **Depends on:** RFC 001, RFC 005
- **Scope:** Capturing the user's active ChatGPT/Claude request and response

## 1. Summary

When a user opens a supported ChatGPT or Claude browser page, Lemon may activate its browser bridge. The bridge captures relevant conversation content, separates user-authored prompts from assistant responses, and creates a task candidate for interpretation and execution.

This capability must be transparent and permissioned. The design must not silently monitor unrelated browsing or assume unrestricted access to provider pages.

## 2. User experience

Proposed default behavior:

1. The user opens a supported ChatGPT or Claude page.
2. Lemon detects the supported domain and shows a clear enabled/disabled status.
3. Lemon reads only the supported page content required for the selected feature, under granted browser permissions.
4. Lemon identifies the latest relevant user request and associated assistant response.
5. A task candidate is created. For ambiguous contexts, Lemon shows a small suggestion or asks the user to select “Run with Lemon.”
6. The task is decomposed and validated.
7. Lemon executes allowed actions and shows status, approval requests, and results.

The product must provide an obvious pause/disable control and per-site controls. “Open ChatGPT” is not, by itself, consent to execute every interpreted instruction.

## 3. Context selection

The bridge should preserve role and order for each conversation turn. It should not assume the latest assistant response is the user's instruction. Assistant output may contain suggestions, examples, quoted messages, or unsafe content that the user did not authorize.

Task-candidate rules should consider:
- Latest user-authored turn.
- The assistant response associated with that turn.
- Explicit user selection or a Lemon invocation.
- Whether the assistant response contains a proposed action or only an explanation.
- Whether the request is already complete or is a hypothetical example.
- Whether multiple plausible active tasks exist.

For early releases, an explicit user gesture is safer than automatically executing any inferred intent. Proactive suggestions can be added after false-positive rates are measured.

## 4. Semantic retrieval

Semantic search can retrieve relevant earlier turns from a bounded, local conversation index. Each retrieved item must retain provenance and timestamp. Retrieval must not convert old instructions into current authorization.

Use retrieval to answer “what context helps interpret this request?” not “what can Lemon do without asking?” If the user asks for a current task, stale details should be confirmed when they materially affect the action.

## 5. Browser integration constraints

- Use supported browser extension permissions and APIs.
- Request host permissions for supported domains only where possible.
- Avoid collecting unrelated tabs, passwords, session tokens, or hidden page data.
- Do not bypass authentication, anti-bot controls, provider restrictions, or access controls.
- Treat page DOM changes as expected; use tests and graceful unsupported-state handling.
- Make it clear when browser content is sent to an external LLM API.
- Do not rely on brittle DOM selectors as the sole long-term integration contract.

The exact approach must be checked against current provider terms and browser-store policies before distribution.

## 6. Data minimization

Capture only the relevant prompt, associated response, and task-specific context. Prefer an in-memory task candidate until the user starts a task. If conversation history is indexed, provide a clear retention setting and deletion controls. Do not transmit the whole conversation when a small excerpt is sufficient.

## 7. Acceptance criteria

- The user can disable capture globally and per supported site.
- The bridge distinguishes user and assistant turns.
- Unrelated tabs are not captured by default.
- A quoted or hypothetical instruction does not automatically execute.
- External API transmission is disclosed and follows configuration.
- DOM changes cause safe failure, not broad page scraping.
- Captured content can be inspected before execution in the initial MVP.
