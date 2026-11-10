# QA Ticket

Adapt this template to the project's language and ticket format. Replace placeholders
with concrete information; repeat the scenario section for each selected scenario.
Leave execution results pending for the testing agent.

Derive expected results from acceptance criteria and approved decisions; use the
implementation to identify risks. Flag ambiguous requirements for clarification rather
than treating current behavior as the expected result.

Default to text evidence for local agent checks: visible text, control states, relevant
network requests and responses, and relevant storage or database reads. Select evidence
that proves each expectation; redact secrets and personal data. For layout or copy named
in the ticket, require showing the relevant screens to the user when available, or a
written description of layout problems when the user is away. Preserve any more specific
project or ticket evidence rules, including screenshot, visual-baseline, installed-app,
or actual-device requirements. Carry the applicable rules into the ticket so its executor
can follow them without loading this skill.

Keep simulated conditions labeled as simulations. If local execution cannot establish a
required fact, describe the local coverage and the remaining device or environment check
separately. A local pass must not imply that the remaining check passed. Use isolated
local data; identify external dependencies and required authorization without treating a
QA ticket as permission to modify shared environments. Keep credentials out of the ticket.

```markdown
# Manual QA: <behavior under test>

## Purpose and scope
- Implementation ticket: <link>
- Target revision or readiness dependency: <revision or dependency>
- Why manual QA is needed: <evidence gap or project requirement>
- Requirements and approved decisions: <references>
- Remaining checks outside local coverage: <checks, or none>

## Local setup
- Run instructions: <commands or maintained guide>
- Services and environment: <required setup>
- Test identity, role, fixtures, and initial state: <safe local prerequisites>
- Reset and cleanup: <steps where scenarios mutate data>
- Known blockers: <missing prerequisites, or none>

## QA-01: <scenario name>
- Requirement or regression risk: <reference and behavior>
- Preconditions: <scenario-specific starting state>
1. <Action with concrete input> — expect <observable result>.
2. <Next action> — expect <observable result>.
- Evidence to record: <observations that establish the expected results>

## Reporting and completion
Record the tested revision and environment. For each scenario, report pass, fail, or
blocked, actual results, and evidence. For failures, include reproduction steps,
expected versus actual behavior, and a defect link or the project's follow-up.
For blocked scenarios, name the missing prerequisite. Report remaining device or
environment checks separately. Distinguish execution complete from QA passed:
unresolved failures and blocked required scenarios remain explicit, and the project's
rules govern accepting or deferring them.
```
