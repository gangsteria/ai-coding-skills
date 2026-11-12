---
name: user-guide
description: User guide upkeep. Use when a finished change alters what users see or do, or when setting the guide up, writing it for existing features, or working through its plan.
---

# User Guide

The guide is a set of **how-to** pages: each takes one user **goal** from start to visible
result, in the words the app shows, so nobody has to explain the app in person. A goal is
one result a user would name when asking for help. Reaching it another way, or once more,
is the same goal; acting later on what a goal produced, such as editing, archiving,
restoring or revoking it, is a goal of its own. So is a run of steps that two or more
goals pass through, such as entering a sign-in code. A screen that only reports a state,
or a control the app shows as unavailable, carries none. A **concept** page holds, once, a
rule that several how-to pages rely on. A goal's **batch** is its how-to page with the
concept pages it is the first to link to, in the order of the index or, for pages the
index does not list yet, of the plan. A **stale** page, one the app now contradicts,
misleads more than a missing one: every change gets the guide searched, and every page is
**walked** in the running app before it ships, or ships reported as unwalked.

Project settings live in `.agents/user-guide.md`. Take the first **route** that fits:

- No settings file? Read [`setup.md`](setup.md) first.
- A new inventory asked for? Read [`sweep.md`](sweep.md) in place of step 1.
- A change to the app to assess, named by the user or just finished in this session?
  Start at step 1.
- A plan named in the settings? Read [`plan.md`](plan.md) in place of step 1.
- No page in the guide location? Take the sweep route: a first guide starts from the
  inventory.
- Otherwise, or when it is unclear which route the user means, say what you found (the
  guide's pages, no open plan, no change to assess) and ask whether to check the whole app
  against the guide. A yes takes the sweep route.

## 1. List what changed for the user

Read the settings, then the change: its diff, ticket and acceptance criteria. List every
difference a user meets inside the **scope** the settings give: screens, control labels,
messages, the steps of a flow, who may do what, and the limits and rules a user runs into.
Group the list by goal, per audience in the settings.

A change that adds an area the scope does not name, or removes the reason an area is out,
settles the scope first: propose to the user whether the area is in or out, and record the
answer in the settings.

Done when: every changed screen, label, message, step, permission, limit and rule in scope
sits under a goal or is marked as carrying none, and a scope question the change raised is
answered in the settings. An empty list ends the work: report `Guide: not needed` with the
reason.

## 2. Decide each goal's outcome

Search the guide for every label, screen and rule on the list. Then give each goal one
outcome:

- **Update:** a page in the goal's batch states something the change made stale. This
  holds however small the change is.
- **Add:** the user reaches a new goal, or an old one by different steps, and the screen
  alone does not carry them through. That is the case when the flow crosses screens, has a
  precondition the user must arrange before the screen lets them through (an assignment,
  an earlier approval, access someone else grants; the role of the goal's own audience is
  already held), has a consequence that is hard to undo, leaves the user waiting on
  something outside the screen (an e-mail, another person, a limit that resets), needs a
  recovery only this flow has, or turns on a domain rule the screen does not state.
- **Fix the UI:** the page would only say what a label means or what to do next. That is a
  copy problem. Propose the wording for the app, in the change at hand when it owns that
  copy and in the report otherwise, and write no page for it. Interface wording belongs to
  `ux-copy`.
- **None:** the screen carries the user through, or the difference is only visual.

Two things are settled across goals:

- **A shared run of steps.** The run is one goal, with an outcome decided like any other;
  a run that is already a goal, as signing in is, stays that goal. Decide each goal that
  passes through it as if the run were a single step, and link its page to the run's page
  when the run has one. A run that becomes shared through a new goal is an Add, and each
  existing page that carries its steps inline is an Update.
- **A shared rule.** A domain rule that two or more how-to pages, existing or planned,
  rely on, and that the screens state only in part or leave unstated, gets one concept
  page, and they link to it. A recovery the app handles the same way in every flow (a
  conflicting edit, an unconfirmed save, a lost connection or session) is such a rule, and
  a goal whose only difficulty is that recovery is a None.

Done when: every goal, shared runs among them, has one outcome with a one-line reason,
every stale statement the search found belongs to an Update, and every rule that calls for
a concept page has its page, existing or planned.

## 3. Write or defer

Count the goals with an Update or Add outcome.

- **No such goal:** go to step 4.
- **Two or more:** **defer** them all. Write none of their pages; read
  [`plan.md`](plan.md), record them in the guide plan, and go to step 4.
- **One:** write its batch as [`page-format.md`](page-format.md) describes, and walk each
  page as [`walk.md`](walk.md) describes before writing the next, a concept page ahead of
  the how-to page that links to it. Then, when the settings give delivery as pending or
  name a plan, read [`plan.md`](plan.md) and bring the plan up to date.

Done when: the one batch is written, in the guide location, in the guide language and
linked from the index, each of its pages meets the completion criterion in `walk.md`, and
the plan is up to date where delivery is pending or a plan exists; or every deferred goal
has its entry in the plan.

## 4. Report

Where the project reports a change (pull request description or ticket), or in the reply
to the user when no change carries the work, write one line per page written or updated
and per Fix the UI proposal, and, when goals were deferred, one line with their number and
the plan. When no goal calls for a page or a proposal, write the not-needed line alone.

```text
Guide: updated <page> — <what changed>
Guide: added <page>
Guide: deferred <number> goals — <plan link, or "plan drafted">
Guide: UI fix proposed — <label> → <wording>
Guide: not needed — <reason>
```

Below these lines, list unwalked pages with their missing prerequisite and unobserved
states with what producing them takes, so a reviewer can tell what was seen from what was
drafted. When a page was written while delivery is pending, add where the pages can be
read until it is built. After assessing a change while the guide holds no other page and
the settings name no plan, add that the features that already exist have no guide yet,
and that invoking the skill with no change to assess starts their inventory.

Done when: every page written or updated and every proposal has its line, deferred goals
have theirs with the plan, and every unwalked page and unobserved state is listed.
