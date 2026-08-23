# COMMAND ROUTER

This file defines how user requests should be routed to commands, agents, and skills.

Always read `AI_RULES.md` first.

---

## CREATE PAGE / FEATURE

Trigger examples:

* create page
* build page
* add new screen
* create module
* create frontend feature
* build user management
* add new workflow

Route:

User Request
→ `create-page`
→ `manager-agent`
→ `frontend-agent`

Frontend Agent may use:

* `frontend-design`
* `ux-reasoning`
* `component-reuse`
* `responsive-design`
* `animation`
* `accessibility`
* `code-quality`

Then:

→ `ui-reviewer-agent`
→ `tester-agent`
→ `code-reviewer-agent`

---

## REVIEW PAGE

Trigger examples:

* review page
* check UI
* check UX
* review responsiveness
* review theme
* check component reuse
* inspect design

Route:

User Request
→ `review-page`
→ `manager-agent`
→ `ui-reviewer-agent`

Use skills:

* `frontend-design`
* `ux-reasoning`
* `responsive-design`
* `accessibility`

If code quality also needs review:

→ `code-reviewer-agent`

Do not modify code unless the user asks for fixes.

---

## FIX PAGE / BUG

Trigger examples:

* fix page
* fix issue
* fix UI
* fix bug
* fix responsiveness
* fix broken interaction

Route:

User Request
→ `fix-page`
→ `manager-agent`
→ `frontend-agent`

Use relevant skills only.

Then:

→ `tester-agent`

If visible UI/UX changed:

→ `ui-reviewer-agent`

---

## REFACTOR

Trigger examples:

* refactor page
* clean code
* split component
* reduce duplication
* improve maintainability

Route:

User Request
→ `refactor-page`
→ `manager-agent`
→ `frontend-agent`
→ `tester-agent`
→ `code-reviewer-agent`

Use `ui-reviewer-agent` only if visible UI changed.

---

## UX-HEAVY TASK

Trigger examples:

* redesign workflow
* improve user flow
* decide best component
* improve interaction
* simplify form
* improve admin UX

Route:

User Request
→ `manager-agent`
→ `ux-reasoning`
→ `frontend-agent`
→ `ui-reviewer-agent`
→ `tester-agent`
→ `code-reviewer-agent`

Do not blindly reuse existing components.

Choose interaction based on context.

---

## RESPONSIVE TASK

Trigger examples:

* mobile issue
* tablet issue
* layout breaks
* overflow issue
* responsive fix

Route:

User Request
→ `manager-agent`
→ `frontend-agent`

Use:

* `responsive-design`
* `frontend-design`
* `ux-reasoning`

Then:

→ `ui-reviewer-agent`

Use `tester-agent` if behavior is affected.

---

## ACCESSIBILITY TASK

Trigger examples:

* accessibility
* keyboard navigation
* focus issue
* aria issue
* contrast issue

Route:

User Request
→ `manager-agent`
→ `frontend-agent`

Use:

* `accessibility`
* `frontend-design`

Then:

→ `ui-reviewer-agent`
→ `tester-agent` if test tooling supports it

---

## CODE REVIEW ONLY

Trigger examples:

* review code
* check code quality
* architecture review
* duplication review
* performance review

Route:

User Request
→ `manager-agent`
→ `code-reviewer-agent`

Use:

* `code-quality`
* `component-reuse`

---

## TEST ONLY

Trigger examples:

* test this page
* run tests
* check flow
* validate feature

Route:

User Request
→ `manager-agent`
→ `tester-agent`

Use:

* `testing`

If test fails:

→ `frontend-agent`
→ `tester-agent` re-check

---

## PACKAGE / COMPONENT DECISION

Trigger examples:

* should we install a package
* add component
* need a new library
* which component should be used

Route:

User Request
→ `manager-agent`
→ `frontend-agent`

Use:

* `ux-reasoning`
* `component-reuse`
* `code-quality`

Before adding any package:

1. inspect existing components
2. inspect installed packages
3. evaluate UX fit
4. evaluate maintenance
5. adapt new package to existing theme

---

## FAILURE ROUTING

If UI Reviewer finds a blocking issue:

`ui-reviewer-agent`
→ `manager-agent`
→ `frontend-agent`
→ targeted fix

If Tester fails:

`tester-agent`
→ `manager-agent`
→ `frontend-agent`
→ targeted fix
→ `tester-agent`

If Code Reviewer finds a blocking issue:

`code-reviewer-agent`
→ `manager-agent`
→ `frontend-agent`
→ targeted fix

Do not restart the full workflow unnecessarily.

---

## GENERAL RULE

Do not call every agent for every request.

Use only the agents and skills relevant to the task.

The Manager Agent is responsible for final routing decisions.

Always follow `AI_RULES.md`.
