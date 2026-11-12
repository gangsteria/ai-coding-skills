# Sweep

Finds the goals of features that already exist. The inventory takes the place of step 1 of
[`SKILL.md`](SKILL.md), and step 2 gains a confirmation by the user; steps 3 and 4 then
run as written.

## Inventory the goals

For each audience in the settings, list every goal the app lets them reach inside the
scope, as the working branch runs it. Check each of these sources:

- the app's navigation and routes;
- every file and ticket among the step sources in the settings;
- the project's spec and glossary;
- the running app, signed in as each audience.

Done when: every screen in scope that each audience can reach maps to a goal or is marked
as carrying none, every step-source file and ticket is accounted for, and the revision
inventoried is noted for the plan.

## Confirm the outcomes

Run step 2 of `SKILL.md` over the inventory. A goal whose page exists is an Update when
the page is stale and a None when it is current. Present the result to the user: each goal
with its audience, outcome and reason, then the concept pages and the Fix the UI
proposals, and what step 3 of `SKILL.md` will do with these outcomes. The user confirms
the list before step 3.

Done when: the user has confirmed every goal's outcome.
