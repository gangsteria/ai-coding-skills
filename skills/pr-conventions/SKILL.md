---
name: pr-conventions
description: Pull request title and ticket-link conventions. Use when opening a pull request or changing its title or description.
---

# PR Conventions

A pull request title is a commit subject in waiting: squash-merging several commits lands it
on the trunk. The description's link lines tie the PR to its ticket, so every ticket's
timeline lists the PRs that worked on it.

## 1. Title

The title is a commit subject describing the whole PR: `<type>: <what changed>`. The subject
format and type vocabulary belong to `commit-conventions`. The title names the work; the
ticket number lives in the link lines.

When the repo configures commitlint (a `commitlint.config.*` file or a `commitlint` key in
`package.json`), lint the title through the repo's package runner before `gh pr create` or
`gh pr edit --title`:

    printf '%s\n' "<title>" | pnpm exec commitlint    # or npx, per the lockfile

Done when: the title passes the repo's commitlint, or, where the repo has none, matches the
subject format.

## 2. Ticket links

End the description with one line per ticket the PR works on:

- `Closes #N` — merging this PR finishes the ticket.
- `Refs #N` — this PR is one part of the ticket; more work follows.

Done when: every ticket the PR works on has exactly one `Closes` or `Refs` line, and a PR
without a ticket has none.
