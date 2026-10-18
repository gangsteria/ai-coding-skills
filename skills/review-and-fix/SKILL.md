---
name: review-and-fix
description: Review a finished diff in rounds, fixing findings until only trivial ones remain; then commit and push. Use when an implementation is finished, before proposing its commits.
---

# Review and Fix

Run a finished change through rounds of review and fixes. Stop when a whole round turns up nothing worth
changing, then deliver the change.

Review is split into two axes run as parallel sub-agents: **Standards** (does the change follow the
repo's documented coding standards?) and **Spec** (does it do what was asked?). When a review skill
exists, such as `code-review`, it owns those two axes. This skill owns the loop around them: how a
finding is labelled, what gets fixed, what each new round is told, when the loop stops, and how the
result lands.

## 1. Pin the change and its spec

- **Fixed point.** Use `HEAD` for an uncommitted diff (`git diff HEAD`). Use the base branch or SHA
  for committed work.
- **Spec.** Find what the change was meant to do, in this order:
  1. the decisions the maintainer agreed in this session, quoted verbatim;
  2. the originating issue;
  3. the PR description.

  Quote any agreed user-facing copy exactly.

- **Repo-specific evidence.** When the project's documented rules require extra evidence for the
  kind of change in the diff (screenshots, a journey run, a generated artifact), produce it now and
  hand it to every review round.

Done when: the diff is non-empty, the spec is written out in full for the reviewers, and any
evidence the project requires for this kind of change is fresh.

## 2. Review round

Run both axes in parallel. Every reviewer labels every finding **trivial** or **non-trivial**:

- **Trivial**: cosmetic, or a nit with no effect on behaviour, readability, test value or what a
  user reads.
- **Non-trivial**: anything else, including any doubt about user-facing wording.

Done when: both reports are in, and every finding carries a label.

## 3. Fix

- Fix every non-trivial finding.
- Fix a trivial finding only when the fix is a one-liner **and** the finding breaks a documented repo
  rule. Leave every other trivial finding alone.
- When a fix changes copy the maintainer approved, keep the approved meaning, and list the old and
  new wording in the final report.
- When a fix would change an agreed decision, do not apply it. Put the decision back to the maintainer.
- Re-run the narrowest checks that cover the touched files (tests, type-check, lint, format check),
  using the project's own commands.

Done when: every non-trivial finding is fixed or escalated, and the narrow checks pass.

## 4. Re-review

Start a new round (step 2). Tell the reviewers:

- what earlier rounds fixed, so they can check that each fix was done correctly;
- what the maintainer chose to keep, so they do not raise it again;
- to report only **new** findings, plus any earlier fix that went wrong.

Loop until one round returns no non-trivial findings on either axis. If the third round still
returns a non-trivial finding, stop and hand the findings to the maintainer, because the change
needs a decision rather than another pass.

Done when: a full round comes back with nothing non-trivial, or the loop was handed to the
maintainer after three rounds.

## 5. Commit and push

- Commit the change following the project's commit conventions.
  - If the maintainer approved the message before the loop and the fixes did not change what the
    diff does, that approval stands.
  - Otherwise, propose the message again from the final diff.
- Push. If the project runs checks on push, a failure there goes back to step 3.
  - When it fails on a test that passes when run alone, run that test file alone to confirm. Then
    retry the push once and report the flake.

Done when: the commit is on the remote branch and the push-time checks passed.

## 6. Report

Open the report with the **tally**: how many rounds ran, and whether the loop ended on a clean round
or was handed over after three. Then list:

- per round, each non-trivial finding and how it was fixed;
- any copy changed, with the old and new wording;
- the trivial findings left alone, one line each;
- any evidence produced in step 1, and whether it caught something the diff alone did not;
- the commit SHA and the push result.

Done when: the maintainer has the tally, and every non-trivial finding from every round is accounted for.
