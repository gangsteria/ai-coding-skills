# Guide plan

The plan is the guide's backlog: every goal with its outcome, and the work still owed as
an ordered list of **entries**. An entry is one session's work: one goal's batch, or the
delivery.

- Deferring goals, or just wrote a batch? Read "Bring the plan up to date".
- Otherwise read "Work the next entry".

## Bring the plan up to date

Give each deferred goal one batch entry. Order the batch entries: Update goals first, then
the order a new user meets the goals: getting into the app, the first core goal, then the
rest. A shared run's entry comes ahead of the first goal that passes through it, so its
page exists when theirs link to it. Note on each what its walk needs beyond the walk setup
in the settings, such as a second account or a state only another person can cause.

While the settings give delivery as pending, the plan carries one delivery entry, placed
where the guide first has a page to show: ahead of every batch entry when a page exists,
otherwise right after the first batch entry.

The plan is one ticket in the project's tracker, in the project's ticket language,
template and labels; page titles stay in the guide language. When the settings name a
plan, add to that ticket: a goal it already holds keeps its entry, and a goal whose batch
this run wrote has its entry ticked. Otherwise create the ticket and record its link in
the settings as `Plan`. When the tracker cannot be written to, return the complete draft,
report it as a draft, and name what blocked it.

Done when: the ticket holds every goal decided in this run with its outcome and revision,
every deferred goal has exactly one batch entry, the delivery entry is there while
delivery is pending, and the settings carry the ticket's link; or the complete draft is
returned with what blocked saving it.

## Work the next entry

Take the first open entry. A delivery entry waits until the guide has a page: while it has
none, take the next batch entry.

- **Delivery:** build what the Delivery setting names; for an in-app route, read
  [`in-app-delivery.md`](in-app-delivery.md). Record delivery as built in the settings,
  tick the entry, and report what was built and where the guide now opens.
- **Batch:** decide the goal's outcome again by the rule in step 2 of `SKILL.md`, from its
  screens in the app as it runs now, and correct the plan where the outcome changed. An
  outcome that leaves nothing to write closes the entry: record the reason on it and
  report it. Otherwise carry on from step 3 of `SKILL.md` through its report.

End the report with the next open entry, which starts from a new request. When no entry
is left open, set `Plan` in the settings to none and report the plan as finished.

Done when: the entry is ticked or closed with its reason in the ticket, the settings give
delivery as built after a delivery entry and `Plan` as none after the last entry, and the
report names the next open entry or the plan as finished.

## Format

```markdown
# User guide plan: <app, or the scope planned>

Settings: `.agents/user-guide.md`.

## Goals

| # | Goal | Audience | Outcome | Reason | Decided at |
| --- | --- | --- | --- | --- | --- |
| 1 | <goal> | <audience> | <Add, Update, Fix the UI or None> | <one line> | <revision> |

Screens carrying no goal: <screens, or none>.

## Entries

- [ ] Batch for goal <#>: <how-to page title>; with it: <concept page titles, or none>. Walk needs: <need, or none>.
- [ ] Delivery: <what the Delivery setting names>.

## Fix the UI proposals

- <screen or label> → <wording>
```
