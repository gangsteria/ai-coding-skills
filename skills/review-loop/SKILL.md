---
name: review-loop
description: Review and fix a finished implementation with code-review. Use when implementation in the current task is complete, before handoff.
---

# Review Loop

Run a finished change through rounds of review and fixes. Stop when a whole round turns up nothing worth
changing, then hand the reviewed diff back to the surrounding task for delivery.

Review is split into two axes: **Standards** (does the change follow the repo's documented coding
standards?) and **Spec** (does it do what was asked?). `code-review` runs those axes in each round.
This skill supplies the current change and context, fixes grounded findings and decides when to stop.

## 1. Pin the change and its spec

- **Fixed point.** Save one commit SHA for the whole loop. For uncommitted work, use
  `git rev-parse HEAD`. To review the branch's commits too, use `git merge-base <base-branch> HEAD`,
  or resolve the supplied base commit to a SHA. Keep this SHA through every round.
- **Change.** In each round, run `git diff <base-sha>` for committed, staged and unstaged changes to
  tracked files. Run `git ls-files --others --exclude-standard` and read each listed file as a new
  file; Git diff does not include untracked files.
- **Spec.** Find what the change was meant to do, in this order:
  1. the decisions the maintainer agreed in this session, quoted verbatim;
  2. the originating issue;
  3. the PR description.

  Quote any agreed product text exactly.

- **Repo-specific evidence.** When the project's documented rules require extra evidence for the
  kind of change in the diff (screenshots, a journey run, a generated artifact), produce it now and
  hand it to every review round.

Done when: the base SHA is saved, the tracked diff or untracked-file list is non-empty, the spec is
written out in full for the reviewers, and any evidence the project requires is fresh.

## 2. Review round

Invoke `code-review` in a fresh subagent for this round. Give it the saved base SHA, the output of
`git log <base-sha>..HEAD --oneline`, the full output of `git diff <base-sha>`, the paths and
contents of every untracked file, the full spec, required evidence, and any earlier fixes or
decisions. Tell it to use the supplied change and spec as its review target and pass the whole brief
to fresh Standards and Spec reviewers. Its standalone `git diff <fixed-point>...HEAD` command misses
worktree changes; its standalone spec
discovery misses decisions from this task. If `code-review` is unavailable or cannot review the
supplied change and spec, stop and report the blocker. Keep its two reports separate, then label
each in-scope finding using [Labels](#labels) and record its ground; list out-of-scope findings
separately.

Done when: both reports are in, every finding is labelled or listed as out of scope, and every
non-trivial finding states its ground.

## 3. Fix

Check the ground of every non-trivial finding. A finding the two axes label differently is checked
as non-trivial. Each non-trivial finding ends one of three ways:

- **Fixed**, when its ground holds, however small or invisible the effect.
- **Rejected**, when its ground rests on a misreading or a scenario that cannot happen. Record the
  proof: the code line, or a test that shows it.
- **Escalated**, when the fix would change an agreed decision. Leave it unfixed and put the decision
  back to the maintainer. An escalation ends this run of the loop until the maintainer decides.

A trivial finding with a ground the reviewer missed becomes non-trivial: name the ground and fix it.
Leave every other trivial finding, and every out-of-scope finding, as it is.

When a fix changes product text the maintainer approved, keep the approved meaning, and list the old
and new wording in the final report.

Re-run the narrowest checks that cover the touched files (tests, type-check, lint, format check),
using the project's own commands.

Done when: every non-trivial finding is fixed, rejected with proof, or escalated, and the narrow
checks pass.

## 4. Re-review

Invoke `code-review` again with a fresh brief (step 2). Include in that brief:

- what earlier rounds fixed, so they can check that each fix was done correctly;
- what the maintainer chose to keep and why, plus every rejected finding with its recorded proof, so
  neither comes back;
- to report only **new** findings, plus any earlier fix that went wrong.

Loop until one round returns no non-trivial findings on either axis. Every fix is reviewed by the
round after it. If the third round still returns a non-trivial finding, stop and hand the findings
to the maintainer, because the change needs a decision rather than another pass.

Done when: a full round comes back with nothing non-trivial, or the loop was handed to the
maintainer for a decision.

## 5. Report and hand off

Open the report with the **tally**: how many rounds actually ran, and whether the loop ended on a
clean round or was handed over for a decision. Then list:

- per round, each non-trivial finding, its ground, and how it ended: fixed, rejected with its proof,
  or escalated; mark the ones you raised from trivial;
- the trivial findings left alone, one line each;
- the out-of-scope findings, as candidates for an issue the maintainer may file;
- any product text changed, with the old and new wording;
- any evidence produced in step 1, and whether it caught something the diff alone did not.

After a clean round, return the reviewed diff to the surrounding task. When a decision is pending,
report the question and wait for the maintainer before delivery.

Done when: the maintainer has the tally, every finding from every round is accounted for, and the
surrounding task has either a reviewed diff or a decision to resolve.

## Labels

Label every finding **trivial** or **non-trivial**. A finding is non-trivial, worth another round of
review, only when it carries one **ground** that can be checked:

- **Consequence**: what goes wrong if the finding ships unfixed, as one of:
  - _behaviour_: a named input or state produces a wrong result;
  - _product text_: text the running product shows someone (UI copy, an error message, an admin
    label, an `aria-label`) is wrong, misleading or unclear;
  - _misleading code_: a name or comment says the code does something it does not;
  - _test value_: a test passes while the behaviour it protects breaks, or behaviour the diff adds
    has no test.

  A cost to some future change counts only when the spec or an open issue names that change.

- **Rule**: a breach of a hard documented repo rule, cited by file and rule. A hard rule says must,
  never or always, or can be checked without judgement. A rule that says prefer, or applies when
  something reads better, needs a consequence as its ground instead.
- **Spec**: a requirement that is missing or done wrong, or behaviour the spec did not ask for, with
  the spec line quoted.

A finding with no ground is trivial: a smell, a naming or style preference, "would read better", a
"could" with no path to it. Product text is the one exception: when unsure whether it reads right,
label it non-trivial.

A finding is **out of scope** when its problem would exist unchanged without this diff. List it
separately, without a label. Lines the diff changed are in scope, and so is any consequence the
diff newly causes, such as new code that exposes an old bug.

| Finding                                                          | Label       | Ground                                      |
| ---------------------------------------------------------------- | ----------- | ------------------------------------------- |
| On touch devices, `blur` overwrites the item the user just chose | non-trivial | behaviour: tap, then blur, loses the choice |
| Creating a record without its parent fails and shows nothing     | non-trivial | behaviour: creation fails silently          |
| A new boundary branch has no test                                | non-trivial | test value                                  |
| `isValid()` also changes its input                               | non-trivial | misleading code                             |
| A middle-man object could be inlined and would read better       | trivial     | none: a smell with no consequence           |
| No comment says why `x !== undefined` is checked                 | trivial     | none: nothing misleads                      |
| Rapid repeats could drop steps, with no sequence named           | trivial     | none: no path to the failure                |
| A line runs past the usual width                                 | trivial     | none: formatting                            |
