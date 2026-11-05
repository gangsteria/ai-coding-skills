---
name: review-loop
description: Review and fix a finished implementation with code-review. Use when implementation in the current task is complete, before handoff.
---

# Review Loop

Run a finished change through rounds of review and fixes. When a whole round finds nothing non-trivial,
finish the trivial cleanup and hand the diff back to the surrounding task for delivery.

Review is split into two axes: **Standards** (does the change follow the repo's documented coding
standards?) and **Spec** (does it do what was asked?). `code-review` runs those axes in each round.
This skill supplies the current change and context, fixes grounded findings and decides when to stop.

## 0. Decide whether to run

The project's documented rules on when to review come first; these defaults apply where it is
silent. Skip the loop when:

- the user asked for no review;
- the change touches only documentation or comments;
- the change is a throwaway prototype;
- the change touches only generated files;
- there is no change;
- the loop already reviewed this exact change and nothing has changed since.

Skip it too when you are a subagent: the reviewers would sit a second level below the task, and
review allows one. Hand back to the agent that delegated to you the spec, every decision you made
beyond it, and the list of changed files, so it can run the loop on your change.

When you skip, say so in one line in your handoff: `Review skipped: <reason>.`

Done when: the loop runs, or the handoff names why it was skipped.

## 1. Pin the change and its spec

- **Fixed point.** Save one commit SHA for the whole loop. For uncommitted work, use
  `git rev-parse HEAD`. To review the branch's commits too, use `git merge-base <base-branch> HEAD`,
  or resolve the supplied base commit to a SHA. Keep this SHA through every round.
- **Change.** In each round, run `git diff <base-sha>` for committed, staged and unstaged changes to
  tracked files. Run `git ls-files --others --exclude-standard` and read each listed file as a new
  file; Git diff does not include untracked files.
- **Spec.** Find what the change was meant to do, in this order:
  1. the maintainer's request and the decisions agreed in this session, quoted verbatim;
  2. the originating issue;
  3. the PR description.

  Quote any agreed product text exactly. When none of these exists, mark the spec as unavailable
  rather than reconstructing it from the diff: the Spec axis then reports `no spec available`, and
  the report says so.

- **Repo-specific evidence.** When the project's documented rules require extra evidence for the
  kind of change in the diff (screenshots, a journey run, a generated artifact), produce it now and
  hand it to every review round.

Done when: the base SHA is saved, the tracked diff or untracked-file list is non-empty, the spec is
written out in full for the reviewers or marked unavailable, and any evidence the project requires
is fresh.

## 2. Review round

Invoke `code-review` yourself for this round, not through a subagent: review allows one level of
subagents, and those are its Standards and Spec reviewers. Use as its review target the saved base
SHA, the output of `git log <base-sha>..HEAD --oneline`, the full output of `git diff <base-sha>`,
the path of every untracked file with an instruction to read each one in full as a new file, the
full spec or its unavailable status, required evidence, and any earlier fixes or decisions,
including those from an earlier run of this loop on the same change. Use this target in place of
`code-review`'s own diff and spec discovery: it covers uncommitted work and the decisions made in
this task.

Pass that whole brief to both fresh reviewers, and tell them not to start subagents of their own.
Run them in parallel where the agent supports it, sequentially otherwise, and wait for both
reports; never leave a reviewer running in the background. A round without both reviewers'
reports, or with a review you wrote in their place, does not count: rerun it once, without counting
the rerun as a round. If the rerun also lacks a report, or `code-review` is unavailable or cannot
review the supplied change and spec, stop and report the blocker. Keep the two reports separate,
then label each in-scope finding using [Labels](labels.md) and record its ground; list out-of-scope
findings separately.

Then choose the next step before touching any file:

- no non-trivial finding on either axis: go to step 5 for final cleanup, including after round three;
- otherwise, this was the third round: go to step 6 and hand all remaining findings over without
  fixes; non-trivial fixes need a later review round, and none follows the third;
- otherwise: go to step 3.

Done when: both reports came from the fresh reviewers, every finding is labelled or listed as out
of scope, every non-trivial finding states its ground, and the next step is chosen; or a blocker is
reported.

## 3. Fix

When a subagent wrote the change and can still be messaged, send it the findings with their
grounds and the decisions so far, and let it check and fix them: it knows why it built the change
as it did. Otherwise, check and fix them yourself.

Check the ground of every non-trivial finding. When both axes report the same problem and only one
labels it non-trivial, treat it as non-trivial. Each non-trivial finding ends one of three ways:

- **Fixed**, when its ground holds, however small or invisible the effect.
- **Rejected**, when its ground rests on a misreading or a scenario that cannot happen. Record the
  proof: the code line, or a test that shows it.
- **Escalated**, when the fix would change an agreed decision. Leave it unfixed and put the decision
  back to the maintainer. An escalation ends this run of the loop until the maintainer decides.

A trivial finding with a ground the reviewer missed becomes non-trivial: name the ground and fix it.
Keep every other in-scope trivial finding pending for step 5, including findings from earlier
rounds that reviewers will not repeat. Leave out-of-scope findings unchanged.

When a fix changes product text the maintainer approved, keep the approved meaning, and list the old
and new wording in the final report.

Re-run the narrowest checks that cover the touched files (tests, type-check, lint, format check),
using the project's own commands. When they still fail after three attempts to fix them, stop and
report the failing check as a blocker instead of trying again.

Done when: every non-trivial finding is fixed, rejected with proof, or escalated, and the narrow
checks pass or a failing check is reported as a blocker.

## 4. Re-review

Invoke `code-review` again with a fresh brief (step 2). Include in that brief:

- what earlier rounds fixed, so they can check that each fix was done correctly;
- what the maintainer chose to keep and why, plus every rejected finding with its recorded proof, so
  neither comes back; carry these over from an earlier run of this loop on the same change too, such
  as the run that escalated or handed over findings the maintainer has since decided;
- to report only **new** findings, plus any earlier fix that went wrong.

Then return to the choice at the end of step 2. Fixes from step 3 are reviewed by the next round;
step 5 is the only cleanup allowed after the third round.

Done when: a full round comes back with nothing non-trivial, or the loop was handed to the
maintainer for a decision.

## 5. Final trivial cleanup

Collect pending in-scope trivial findings from every round. Before editing, check each against the
current diff: confirm it still applies and that a small, local change improves the code while
preserving behaviour and agreed decisions. Accept every finding that passes this check; record a
brief reason for each one skipped (for example, already resolved, unsupported, or out of scope).
A speculative failure without a concrete path remains unsupported.

If this check reveals a ground from [Labels](labels.md), promote the finding to non-trivial before
making any cleanup edits and return to the choice in step 2 with the updated findings, keeping the
round count. That sends rounds one and two through fixes and re-review, and round three to handoff
without fixes.

Once all pending findings are checked and none was promoted, apply the accepted trivial fixes.
Use the narrow checks and failure limit from step 3. When they pass, go to step 6 without another
review round; when they remain failing, report the blocker. These final trivial fixes are the
exception to re-review: report them as checked but not re-reviewed.

Done when: every pending trivial finding is fixed or skipped with a reason and checks pass, or
promoted findings return to step 2, or a failing check is reported as a blocker.

## 6. Report and hand off

Open the report with the **tally**: how many rounds actually ran, and whether the loop ended on a
clean round, completed final trivial cleanup, was handed over for a decision, or hit a blocker.
Then list:

- per round, each non-trivial finding, its ground, and how it ended: fixed, rejected with its proof,
  escalated, or handed over (found in the third round and left unfixed, with the proposed fix as
  text); mark the ones you raised from trivial;
- each trivial finding and its outcome: fixed in final cleanup (checked but not re-reviewed),
  skipped with a reason, or pending at handoff; include check results and any blockers;
- the out-of-scope findings, as candidates for an issue the maintainer may file;
- any product text changed, with the old and new wording;
- when the spec was unavailable, say so;
- any evidence produced in step 1.

After a clean round and successful final cleanup, return the diff to the surrounding task. When a
decision or blocker is pending, report it and wait for resolution before delivery.

Done when: the maintainer has the tally, every finding from every round is accounted for, and the
surrounding task has either the checked diff or a decision or blocker to resolve.
