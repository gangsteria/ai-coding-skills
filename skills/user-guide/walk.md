# Walk

A walk performs a page in the running app, so that everything the page says has been
seen.

## Set up

Start the app and sign in by the walk setup in the settings, on the viewport the settings
name. When the browser at hand cannot reach that viewport, walk headless at it with a
browser automation script, and say in the report that the walk ran headless. A script
waits for each screen as a person would, until its form is ready and its validation has
settled: a state that only a script's timing produces is not a finding. When the walk
needed something the walk setup lacks, such as an account for an audience or the place a
sign-in message is read, add what worked to the settings.

## How-to pages

Perform each step exactly as written. A page for several audiences is walked in full as
one of them, then as each other audience for the steps and results that differ.

Produce each state under "If something else happens" that the walk setup can produce. A
state it cannot produce is **unobserved**: check its labels against the source and name
what producing it takes.

## Concept pages

Observe each statement in the running app, or trace it to the spec or decision that sets
the rule.

## What the walk finds

- **The page is wrong:** correct the page and perform the corrected step.
- **The screen is wrong:** a step where the screen leaves the reader without the next
  action is a Fix the UI finding. Write the page for the app as it runs and report the
  proposed wording.
- **The flow is broken:** a defect leaves the flow's result wrong or unsafe, such as a
  save the app accepts that breaks what others see, or an action that does more than its
  label says. Open a bug ticket in the project's tracker. When the page can steer the
  reader past the defect, write it for the app as it runs and name the ticket in the
  report; otherwise **park** the batch: write none of it, and mark its plan entry blocked
  by the ticket.
- **The environment cannot walk the page:** the page is **unwalked**. Check its labels
  against the source and name the missing prerequisite.

Done when: each step was performed, and each step or result that differs by audience was
performed as each of those audiences; each quoted label matches the visible text character
for character; each stated result was observed; each other state was observed or reported
as unobserved; and each concept statement was observed or traced. Or the page is unwalked,
with its labels checked and its missing prerequisite named. Or the batch is parked behind
a bug ticket.
