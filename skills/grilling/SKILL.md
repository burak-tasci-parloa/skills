---
name: grilling
description: Stress-test a plan, decision or idea through dependency-aware interview rounds with mandatory interactive question cards. Use when the user asks to be grilled or to challenge their thinking, or runs /grilling.
license: MIT
metadata:
  version: "1.0.0"
---

# Grilling

Interview the user until the material decisions are understood. Map the plan as a design tree: each decision has prerequisites and may unlock further decisions. Use the user's language, be direct and challenge weak assumptions without repeating settled questions.

## Mandatory question cards

Every question you ask the user, including follow-ups and the closing confirmation, must be submitted through a real interactive question-card tool.

- In Claude, call `AskUserQuestion`. In another host, use its available equivalent that renders question cards, such as `request_user_input_async` or `request_user_input`, only where the host permits that tool. Inspect the actual tool schema and respect its limits; never invent parameters or change modes to bypass a restriction.
- Give each question a stable Q number and a short, self-contained title and prompt. Provide **2 or 3 substantive choices** with concise trade-offs. Put the best-supported choice first and mark it **(Recommended)**. If information is insufficient, recommend a concrete fact-finding or deferral choice rather than inventing confidence.
- Every card must allow **Other / a custom free-text answer**. Use the host's built-in Other/free-text field when present; do not add a duplicate option. If absent, add Other and collect the custom answer through the card tool. If the tool cannot capture free text, it cannot satisfy this protocol.
- Never replace a card with Markdown questions, a numbered list, a code block containing a proposed payload, or a text-only "Which option?" prompt. Do not print the same questions alongside the tool call.
- If no permitted tool can display the required cards, briefly state that interactive question cards are unavailable in this session and pause the interview. Do not fall back to prose questions or pretend a card was shown.

The card is mandatory; the exact API name and payload depend on the host. An actual tool call is required to present it. Silence, a preselected recommendation, a timeout or an empty result is not an answer.

## Work the decision frontier

1. Reuse the user's supplied context and accepted decisions. Identify the outcome and map the material decisions and their prerequisites. Do not manufacture branches that cannot affect the plan.
2. Find available facts yourself with the host's permitted tools. Use a subagent only when delegation is available and authorized; otherwise investigate directly. Do not ask the user to supply facts you can retrieve. An unfinished investigation remains an unsettled prerequisite.
3. The **frontier** contains decisions whose prerequisites are settled. Ask only these questions. A question depending on another unanswered question belongs to a later round.
4. Present the frontier through question cards, within the tool's per-call limit. Keep overflow pending and reconsider it after answers arrive. Do not force the user to answer every branch sequentially when independent questions can share a card batch.
5. Wait for actual answers before choosing any dependent branch. With an asynchronous question tool, continue only independent fact-finding while the cards are pending; do not send the same unanswered cards again. Accept custom answers and update the plan from what the user actually said.
6. After each reply, recompute the frontier. Resolve material ambiguity with another card. Keep facts, assumptions, proposals, user decisions and explicitly deferred items distinct.

Be relentless about consequential gaps, not exhaustive about irrelevant details. Preserve recommendations as recommendations until the user answers. An explicit decision to defer settles the current choice to defer; it does not settle the underlying question.

## Close the interview

When no material branch remains silently assumed, give a concise account of the decisions, unresolved or explicitly deferred items, and the next step. Then request confirmation of that shared understanding through a final card, for example with choices to confirm, correct an item or reopen a decision, plus the native Other/free-text field.

Do not act on newly proposed decisions until the user confirms the understanding. Confirmation settles the interview; execute only within the user's existing authorization and the host's permissions. Do not request separate approval for actions the user has already authorized.
