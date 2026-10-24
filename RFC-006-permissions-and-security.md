# RFC 006: Permissions & Security

- **Status:** Proposed
- **Depends on:** RFC 002, RFC 003, RFC 004
- **Scope:** Trust boundaries, authorization, secrets, and safe execution

## 1. Summary

Lemon can act on a user's behalf, so permission and security controls are core product behavior rather than later polish. This RFC establishes least privilege, explicit authorization for consequential actions, secure secret handling, and auditable execution.

## 2. Security objectives

- Prevent untrusted content or model output from independently authorizing actions.
- Limit each adapter to the minimum permissions and resources needed.
- Require confirmation for consequential or irreversible actions.
- Keep credentials and sensitive content out of ordinary logs.
- Provide task visibility, cancellation, and a reviewable action history.
- Fail safely when authorization or target identity is uncertain.

## 3. Trust boundaries

Treat the following as separate trust domains:

1. **User intent:** the request and explicit preferences provided by the user.
2. **Assistant/model output:** helpful but potentially incorrect or manipulated.
3. **External content:** webpages, email bodies, documents, and application text; always untrusted.
4. **Lemon core:** validates schemas, task state, policy, and permissions.
5. **Adapters:** privileged components that interact with platforms or data.
6. **External providers:** subject to their own security, policy, and availability constraints.

Neither a model nor a webpage may grant itself new permissions. All actions must pass through the Lemon policy gate.

## 4. Risk-based authorization

A suggested initial classification:

| Risk class | Examples | Default policy |
|---|---|---|
| Read-only / low | Read an explicitly selected page, summarize a file within granted scope | Allow within granted scope; disclose collection as appropriate |
| Reversible / moderate | Create a draft, add a non-sensitive reminder, organize files in a user-selected folder | Allow if scoped and configured; ask when ambiguity matters |
| Consequential / high | Send an email, publish content, share a file, change account settings | Show target and action; require explicit confirmation unless a carefully scoped user policy explicitly allows it |
| Irreversible / sensitive | Delete important data, make a purchase, transfer funds, disclose secrets, change security controls | Require explicit, action-specific confirmation; deny unsupported or unsafe operations |

This table is a baseline. A platform's own confirmation requirements and legal obligations still apply. Do not infer consent from a previous unrelated approval.

## 5. Permission model

- Use least privilege for filesystem, browser, notification, calendar, and device access.
- Request permissions only when a feature needs them.
- Explain the purpose of each permission in user-facing language.
- Keep permissions scoped by platform, resource, and action where possible.
- Support revocation and communicate the effect of revocation on active automations.
- Separate read and write scopes where the platform supports it.
- Never bypass authentication, access controls, CAPTCHAs, or operating-system permission prompts.

## 6. Credential and secret handling

- Use OS-provided secure storage or an established secrets facility.
- Never hardcode API keys, session cookies, or passwords in source code.
- Never put secrets in prompts, analytics events, crash reports, or general task logs.
- Redact sensitive fields from screenshots, traces, and error messages where feasible.
- Prefer provider-supported authentication flows and scoped tokens.
- Provide a clear disconnect/revoke path.
- Do not extract credentials from browser storage or attempt to circumvent provider controls.

## 7. Prompt injection and content isolation

External text may contain instructions such as “ignore previous rules” or “send this file elsewhere.” Treat these as content, not authority.

Controls:

- Preserve the original user goal separately from retrieved content.
- Label external content as untrusted in internal task context.
- Require typed actions and schema validation.
- Enforce destination, data-scope, and risk checks independently of the model.
- Require fresh authorization before disclosing sensitive information or performing consequential actions.
- Test with malicious pages, emails, documents, and hidden text.

## 8. Data minimization and retention

- Collect only context needed for the task.
- Prefer references to local files or pages over copying entire contents into persistent records.
- Keep audit logs useful but sanitized.
- Define retention and deletion behavior before shipping persistent cloud storage.
- Make it clear whether task data stays local or is sent to an external model/provider.
- Do not assume that local execution means all data remains local if a remote assistant is used.

## 9. Computer-use safety

- Restrict actions to approved adapters and supported APIs.
- Do not run arbitrary model-generated shell commands as a generic fallback.
- Use scoped filesystem roots and path validation.
- Detect unexpected target changes before sensitive actions.
- Use visible previews for sends, purchases, deletions, and sharing operations.
- Treat screenshots and UI text as potentially sensitive.
- Stop and request user intervention when a login, human verification, or unclear security prompt is encountered.

## 10. Auditability and user control

For each task, retain a sanitized record of:

- The user request or a privacy-preserving reference to it.
- The plan version and policy version.
- Actions attempted and their outcomes.
- Approval decisions and timestamps.
- Verification evidence references.
- Errors and retries.
- Pause, cancellation, and revocation events.

Users should be able to inspect active tasks and cancel eligible actions. A cancellation request must prevent future actions as quickly as possible; the UI must not imply that an already-completed external side effect has been undone.

## 11. Threat model and mitigations

| Threat | Mitigation |
|---|---|
| Malicious webpage instructs Lemon to exfiltrate data | Treat page content as untrusted; policy gate and explicit destination checks |
| Model emits malformed or unexpected actions | Strict typed schema, action allowlist, deterministic validation |
| Duplicate email or purchase after timeout | Idempotency where supported; inspect outcome before retry |
| Credential leakage in logs | Secret redaction, secure storage, logging tests |
| Overbroad filesystem access | Scoped roots, canonical path validation, permission prompts |
| Automation continues after user changes their mind | Durable cancellation, task visibility, cancellation propagation |
| False success from UI action | Verify postconditions and report uncertainty honestly |
| Provider or platform restrictions | Use supported interfaces; stop rather than bypass controls |

## 12. Security acceptance criteria

- No action executes before schema validation and policy evaluation.
- High-risk actions require the configured confirmation path.
- Revoked permissions block subsequent actions.
- Secrets are excluded from ordinary logs and error messages.
- Malicious external content cannot expand permissions or change the task's authorized goal.
- Ambiguous outcomes for non-idempotent actions do not trigger blind retries.
- Users can inspect and cancel active tasks.
