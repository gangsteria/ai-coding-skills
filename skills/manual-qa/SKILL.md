---
name: manual-qa
description: QA planning. Use after implementation and automated checks, before closing the implementation ticket, or when assessing manual-testing needs earlier or preparing a local QA handoff for an agent.
---

# Manual QA

Assess the current implementation ticket and, when manual verification is warranted,
create a linked QA ticket that another agent can execute locally. This workflow plans
verification; executing the scenarios is a separate task.

## 1. Establish the verification contract

Read the current ticket, its acceptance criteria and linked decisions, the available
implementation diff, and relevant automated tests. Inspect the host project's agent
instructions, local run guide, issue conventions, and evidence requirements.

Resolve conventions from documented project rules first, then this skill's defaults,
then workstation or agent defaults that allow project-specific choices. Mandatory
higher-priority instructions still apply. Surface incompatible requirements.

Identify the implementation revision or branch, affected behavior, automated coverage,
and available local setup. If implementation is unfinished, mark the assessment
provisional and make the QA ticket depend on the implementation being ready. Resolve a
missing ticket from the current branch or conversation; ask only if the target remains
ambiguous.

Record available automated-check results and the revision they tested. Treat missing or
outdated results as uncertainty and failures as unresolved findings; test existence alone
does not establish successful verification of the assessed revision.

Done when: the acceptance criteria and changed behavior are accounted for, and missing
inputs are distinguished from missing test coverage.

## 2. Decide whether manual verification adds evidence

Choose one outcome and give a short rationale tied to the ticket:

- **Required:** project or ticket rules require it, or a material acceptance criterion
  remains unverified by existing automated checks and benefits from direct observation.
  Examples include layout or copy in context, a multi-step interaction, focus or keyboard
  behavior, session transitions, or integration behavior across real local components.
- **Not needed:** relevant behavior is adequately covered by existing automated checks
  and no applicable rule requires manual evidence. Documentation-only changes and internal
  refactors often qualify, but assess their actual impact.
- **Undetermined:** missing requirements, implementation details, or coverage evidence
  prevent a defensible decision. Name the missing input rather than treating uncertainty
  as a pass or inventing a QA scope.

For Required, identify the smallest set of scenarios that closes the evidence gaps.
Include failure, recovery, access, or persistence cases only where the change makes them
relevant. A missing automated test alone is not a reason to replace it with manual QA;
identify any automated coverage work separately.

Done when: every proposed scenario maps to a requirement or concrete regression risk.
For Not needed, report the rationale and stop without creating a ticket. For Undetermined,
request the missing input and leave the decision open.

## 3. Prepare an executable local handoff

When the assessment is Required, read [QA ticket format and evidence rules](qa-ticket.md)
and draft one linked QA ticket using that reference. Match the project's language,
template, labels, and dependency conventions.

Done when: another agent can reproduce every scenario and judge its result without
guessing inputs, expected behavior, or evidence, and any unavailable prerequisites are
explicit blockers rather than invented setup instructions.

## 4. Publish and verify the handoff

Check for an existing QA ticket linked to this implementation before creating another;
reuse a matching handoff. When implementation or scenarios change, preserve previous
results as history with their tested revision, and mark affected scenarios as requiring
retesting. Their previous pass does not verify the changed behavior.

Use the project's issue workflow and available tracker tools to publish the prepared
handoff, applying its relationship, label, and readiness conventions.
If publication is blocked by missing tracker access, configuration, or authorization,
return the complete draft and exact blocker; distinguish a draft from a created ticket.
Before retrying an uncertain publication result, check whether the ticket already exists.

Verify the saved ticket contains the parent link, local prerequisites, every scenario,
expected results, evidence rules, and completion criteria. Check its metadata against
the resolved project conventions. Report the assessment and ticket URL, or the draft and
publication blocker. Planning or publishing the ticket is not evidence that QA passed.
