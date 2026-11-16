# Guide plan

The plan is the guide's backlog: every goal with its outcome, and the work still owed as
an ordered list of **entries**. An entry is one session's work: one goal's batch, the
delivery, or the closing audit.

- Deferring goals, or just wrote a batch? Read "Bring the plan up to date".
- Otherwise read "Work the next entry".

## Bring the plan up to date

Give each deferred goal one batch entry. Order the batch entries: Update goals first, then
the order a new user meets the goals: getting into the app, the first core goal, then the
rest. A shared run's entry comes ahead of the first goal that passes through it, so its
page exists when theirs link to it. Note on each what its walk needs beyond the walk setup
in the settings, such as a second account or a state only another person can cause.

The plan's last entry is an **audit**, performed under "Work the next entry". Add it when
the plan is created or an existing plan lacks it, and keep it last. Reopen it when new
work is added after an earlier audit.

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
delivery is pending, the audit is last, and the settings carry the ticket's link; or the
complete draft is returned with what blocked saving it.

## Park a batch

Use "Bring the plan up to date" to create or update the plan even for a single goal with
`Plan: none`. Keep the goal's Add or Update outcome, and add or reopen its batch entry,
unchecked and blocked by the bug ticket or draft. Keep the audit open after it. When the
tracker cannot be written to, include the blocked entry in the returned plan draft.

Done when: the batch has one open entry with its blocker in the plan or complete draft,
and any failure to save the bug or plan is named in the report.

## Work the next entry

Take the first executable open entry. A parked batch stays blocked until its bug is fixed
in the running app; then clear the blocker and retry the batch through its normal walk.
Skip blocked entries and entries that need their unfinished work, such as a page linking
to a parked shared run. A delivery entry waits until the guide has a page. If no entry
can run, leave them open, report each blocker or unmet prerequisite, and stop.

- **Delivery:** build what the Delivery setting names; for an in-app route, read
  [`in-app-delivery.md`](in-app-delivery.md). Record delivery as built in the settings,
  tick the entry, and report what was built and where the guide now opens.
- **Batch:** decide the goal's outcome again by the rule in step 2 of `SKILL.md`, from its
  screens in the app as it runs now, and correct the plan where the outcome changed. An
  outcome that leaves nothing to write closes the entry: record the reason on it and
  report it. Otherwise carry on from step 3 of `SKILL.md` through its report.
- **Audit:** runs once every other entry is closed and any changes they produced have
  merged. Open Fix the UI tickets do not block it: audit the app as it runs and name each
  outstanding ticket and the pages it affects in the report. Their fixes recheck those
  pages through the normal change route in `SKILL.md`. Read the index and every page in
  its order as a reader new to the app, checking all of [`page-format.md`](page-format.md)
  and consistent names for controls, states and screens across pages. Walk every page as
  [`walk.md`](walk.md) describes and, with generated screenshots, regenerate every image.
  Fix each page error, report each Fix the UI proposal and bug ticket, and report the
  skill's own shortfalls with a proposed change each. If a walk parks a batch, its reopened
  entry keeps the audit open for a later pass. Otherwise tick the audit after every page
  meets the walk's completion criterion and every finding is fixed or reported.

End the report with the next executable entry, which starts from a new request, or the
blockers when none can run. When no entry is left open, set `Plan` in the settings to none
and report the plan as finished.

Done when: the entry is ticked or closed with its reason, or left open with its blocker;
the settings give delivery as built after a delivery entry and `Plan` as none after the
last entry closes; and the report names the next executable entry, the blockers, or the
plan as finished. A complete draft with its saving blocker named stands in for an update
the tracker could not save.

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
- [ ] Audit: read the guide in the index's order and walk every page again; closes the plan.

## Fix the UI proposals

- <screen or label> → <wording>
```
