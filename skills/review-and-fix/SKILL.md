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

  Quote any agreed product text exactly.

- **Repo-specific evidence.** When the project's documented rules require extra evidence for the
  kind of change in the diff (screenshots, a journey run, a generated artifact), produce it now and
  hand it to every review round.

Done when: the diff is non-empty, the spec is written out in full for the reviewers, and any
evidence the project requires for this kind of change is fresh.

## 2. Review round

Run both axes in parallel, and give every reviewer the [Labels](#labels) section verbatim.

Done when: both reports are in, every finding is labelled or listed as out of scope, and every
non-trivial finding states its ground.

## 3. Fix

Check the ground of every non-trivial finding. A finding the two axes label differently is checked
as non-trivial. Each non-trivial finding ends one of three ways:

- **Fixed**, when its ground holds, however small or invisible the effect.
- **Rejected**, when its ground rests on a misreading or a scenario that cannot happen. Record the
  proof: the code line, or a test that shows it.
- **Escalated**, when the fix would change an agreed decision. Leave it unfixed and put the decision
  back to the maintainer.

A trivial finding with a ground the reviewer missed becomes non-trivial: name the ground and fix it.
Leave every other trivial finding, and every out-of-scope finding, as it is.

When a fix changes product text the maintainer approved, keep the approved meaning, and list the old
and new wording in the final report.

Re-run the narrowest checks that cover the touched files (tests, type-check, lint, format check),
using the project's own commands.

Done when: every non-trivial finding is fixed, rejected with proof, or escalated, and the narrow
checks pass.

## 4. Re-review

Start a new round (step 2). Tell the reviewers:

- what earlier rounds fixed, so they can check that each fix was done correctly;
- what the maintainer chose to keep and what was rejected, with the reason, so neither comes back;
- to report only **new** findings, plus any earlier fix that went wrong.

Loop until one round returns no non-trivial findings on either axis. Every fix is reviewed by the
round after it. If the third round still returns a non-trivial finding, stop and hand the findings
to the maintainer, because the change needs a decision rather than another pass.

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

Open the report with the **tally**: how many rounds actually ran, and whether the loop ended on a
clean round or was handed over after three. Then list:

- per round, each non-trivial finding, its ground, and how it ended: fixed, rejected with its proof,
  or escalated; mark the ones you raised from trivial;
- the trivial findings left alone, one line each;
- the out-of-scope findings, as candidates for an issue the maintainer may file;
- any product text changed, with the old and new wording;
- any evidence produced in step 1, and whether it caught something the diff alone did not;
- the commit SHA and the push result.

Done when: the maintainer has the tally, and every finding from every round is accounted for.

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
