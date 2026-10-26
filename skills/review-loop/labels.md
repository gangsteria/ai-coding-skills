# Labels

Label every finding **trivial** or **non-trivial**. A finding is non-trivial, worth another round of
review, only when it carries one **ground** that can be checked:

- **Consequence**: what goes wrong if the finding ships unfixed, as one of:
  - _behaviour_: a named input or state produces a wrong result;
  - _product text_: text the running product shows someone (UI copy, an error message, a form
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
