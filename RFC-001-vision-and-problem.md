# RFC 001: Lemon — Vision, Problem & Product Philosophy

- **Status:** Proposed
- **Type:** Product / strategy
- **Audience:** Founders, contributors, early developers

## 1. Abstract

Lemon is an open-core AI execution and automation layer that gives users' preferred AI assistants the ability to perform real-world tasks on their devices and across the web.

Users can continue using ChatGPT, Claude, or another supported assistant. Lemon bridges the gap between an assistant's response and actual execution by enabling browser interaction, device control, task automation, and persistent workflows.

Lemon's two foundational capabilities are:

1. **Execution:** turn an AI-generated intention into actions, such as preparing an email, creating a reminder, or working with files.
2. **Automation:** let users define tasks that continue over time, such as monitoring product prices, researching hotels, or notifying users when specified conditions are met.

Lemon is not intended to replace existing AI assistants. It provides a reusable execution layer around them.

## 2. The problem

### 2.1 AI can explain what to do without doing it

AI assistants can draft emails, research products, explain problems, and plan workflows. Yet users often have to leave the conversation and perform the actions manually.

For example, an assistant may write a good email to an HR manager, but the user still has to open an email client, locate the recipient, copy the draft, review it, and send it. Similar friction exists for reminders, browser workflows, and file operations.

The core problem is the gap between **an answer** and **a completed task**.

### 2.2 Agentic capabilities are fragmented and constrained

Some AI products offer computer-use agents, browser automation, scheduled tasks, or integrations. These capabilities may be subject to subscriptions, usage limits, platform restrictions, or provider-specific workflows.

Users can be pushed toward a separate product or forced to change their preferred assistant just to gain execution capabilities.

Lemon proposes a shared execution layer that can work with supported assistants instead of tying execution exclusively to one provider.

### 2.3 Useful work often continues after the prompt

Many tasks require persistence rather than a one-time response:

- Monitor a product and notify me if its price falls below ₹5,000.
- Find suitable hotels near a beach using my dates, budget, and preferences.
- Check a webpage periodically for a meaningful change.
- Run a recurring workflow and report results.
- Remind me about an event at a specified date and time.

These tasks require scheduling, durable state, condition evaluation, and notifications.

### 2.4 Users should not have to rebuild their workflow around every tool

People may use one assistant for coding, another for research, and another for writing. They should not have to abandon those preferences to perform device actions.

Lemon should offer a consistent execution interface across supported assistants and tools, with minimal setup and without requiring an individual app integration wherever a supported browser or device interaction method can do the job.

## 3. Product vision

Lemon aims to make supported AI assistants capable of getting useful work done on a user's behalf.

The user describes an outcome in natural language. Lemon identifies the intended task, obtains missing information, creates a structured plan, checks permissions, executes through available interfaces, verifies the result, and reports what happened.

For persistent tasks, Lemon stores the workflow and evaluates it until it completes, expires, or is cancelled.

> **Core philosophy: Let the user choose the intelligence. Let Lemon provide the execution.**

## 4. Core capabilities

### 4.1 AI-powered execution

Example: “Write an email to my HR manager explaining that I cannot attend today's meeting.”

Expected flow:

1. Capture the user's task and relevant assistant output through a supported integration.
2. Identify the intended outcome, recipient, and required details.
3. Draft or extract the message content.
4. Prepare the message in the appropriate application.
5. Show the user a review step when the action is consequential or authorization is unclear.
6. Send only when the applicable authorization policy is satisfied.
7. Report the actual result, distinguishing success from an attempted action.

Other examples include creating reminders, browser interaction, file management, and supported workflows in applications.

### 4.2 Persistent automation

Example: “Watch this product. Notify me if its price drops below ₹5,000.”

Expected flow:

1. Identify the product and a reliable source.
2. Store the target condition and checking policy.
3. Schedule checks at a reasonable interval.
4. Read and validate the price.
5. Notify the user if the condition is met.
6. Allow the user to pause, edit, or cancel the task.

“Find good hotels near a beach” can be a one-shot research task. Ongoing price or availability monitoring should be an explicit, separate automation.

### 4.3 Cross-assistant compatibility

Lemon should support multiple assistants through maintainable, permitted integration methods. It should not assume unrestricted access to a provider's internal systems or depend on undocumented interfaces without clearly treating them as fragile.

### 4.4 Persistent task management

Tasks should have identifiable states: pending, awaiting approval, running, completed, failed, paused, or cancelled. Users should be able to inspect active work and stop it.

## 5. Usage-limit philosophy

Lemon aims to reduce the marginal cost of computer-use actions for users who already have access to a supported assistant. It should not impose an artificial per-action Lemon quota on local execution where practical.

The distinction is:

- **Model intelligence:** provided by the user's chosen assistant or another supported model.
- **Execution:** provided by Lemon through local and external interfaces.
- **Automation:** managed by Lemon's task engine.
- **Infrastructure:** used only where needed and subject to provider and resource limits.

Lemon cannot promise unlimited model inference, unrestricted third-party access, or unlimited cloud resources. Provider policies, quotas, network access, and device availability still apply. The product must describe these boundaries honestly.

## 6. Differentiation

| Category | Primary role | Lemon's intended distinction |
|---|---|---|
| AI assistants | Understand requests and generate responses | Help turn supported requests into actions |
| Computer-use agents | Perform tasks in a controlled environment | Reusable execution layer across assistants |
| Automation platforms | Connect triggers, actions, and services | Natural-language task creation plus device context |
| App integrations | Expose selected service capabilities | Browser/device interaction where appropriate direct integrations do not exist |

Lemon's differentiation depends on reliability, low setup friction, persistence, portability, and user control, not simply on claiming to support more tools.

## 7. Product principles

1. **Assistant agnosticism:** do not require users to abandon a supported assistant.
2. **Execution over imitation:** focus on completing tasks rather than recreating every chatbot UI.
3. **Minimal setup:** avoid requiring every application to be connected individually when supported alternatives exist.
4. **Local-first where practical:** prefer on-device execution for suitable tasks and disclose cloud dependencies.
5. **Persistence:** support workflows that continue beyond the initial prompt.
6. **User control:** make active tasks visible, pausable, and cancellable.
7. **Permission before consequential actions:** require appropriate authorization for sensitive or irreversible operations.
8. **Verifiability:** distinguish requested, attempted, and confirmed actions.
9. **Open-core development:** keep core execution modular and auditable while allowing sustainable optional services.
10. **Honest limitations:** do not promise access, capabilities, or unlimited resources the system cannot guarantee.

## 8. Non-goals

The initial product will not:

- Train or host its own frontier model.
- Replace ChatGPT, Claude, or other supported assistants.
- Guarantee compatibility with every app or operating system.
- Bypass authentication, usage restrictions, or security controls.
- Execute consequential actions without appropriate authorization.
- Guarantee automation when devices, websites, or networks are unavailable.

## 9. Initial success criteria

- A new user can configure a supported assistant workflow with minimal setup.
- A supported device task can be completed without manually repeating each step.
- Scheduled automation persists across sessions and notifies users when conditions are met.
- Users can inspect, pause, and cancel tasks.
- Failures and partial completion are reported accurately.
- The execution layer remains modular enough to add assistants and platforms.

## 10. Summary

Lemon exists because the path from AI understanding to real execution remains fragmented. By combining assistant compatibility, computer/browser interaction, and persistent automation, Lemon aims to make AI more useful in everyday workflows.

**Lemon is the execution and automation layer between the AI you choose and the work you need done.**
