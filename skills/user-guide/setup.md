# Setup

Runs once per project. Open by telling the user that the settings come first and the work
asked for follows them; with no change waiting to be assessed, that work is an inventory
of the app and a guide plan, with the pages written later, one goal per session.

Inspect the project for every setting in the table, then put them to the user in one
message. A candidate is one whole value for a setting: several audiences or several step
sources found together are one candidate.

- **Ask** Scope and Delivery always, any setting for which the project gives two or more
  candidates, and any setting left without a value because nothing was found and its
  default names nothing.
- **Show** every other setting with its one detected value, or its default when nothing
  was found, and with where the value comes from, for the user to correct.

## Asking

Write the questions in the language the user writes in. Each question has:

- one plain sentence on what the setting decides, taken from the table;
- two to four options to pick from, each with what follows from picking it. The first is
  the recommended one, marked as such and with its reason: the default, or else the
  candidate that best matches how the app is used. A setting without a value has no
  options to offer and is asked openly;
- a last option for an answer in the user's own words, labelled "Other" in the user's
  language. Leave it out when the agent's way of offering choices adds one itself.

Offer the options through the agent's own way of asking for a choice when it has one,
otherwise as a numbered list.

## Settings

| Setting | Decides | Detect from | Default |
| --- | --- | --- | --- |
| Language | The language of the pages and how they address the reader | The app's UI copy, and its form of address where the language has formal and informal forms | The detected language and form of address |
| Location | Where the page files live | An existing folder of user-facing docs | `docs/guide/` |
| Scope | Which parts of the app get pages | The app's areas and what a user can change in each | Areas where a user changes data or acts in a role |
| Audiences | Whose goals the guide covers | Roles in the access rules, glossary or spec | One audience: every signed-in user |
| Viewport | The screen size pages are checked on | The device the app is designed for | The app's smallest supported viewport |
| Walk setup | How the app is started and signed into to check a page | The run guide, seed and fixture files, and how the app delivers sign-in messages locally | The project's run guide |
| Delivery | Where readers open the guide | An existing help route or link in the app | In-app guide route with a navigation link |
| Step sources | Where the ordered steps of a page come from | Browser specs, QA tickets, scenario documents | The change and the running app |
| Screenshots | Whether pages carry images | A browser suite that can capture screens | None: pages are text |
| Terms | Where the app's own words are read from | Where the UI's words for its concepts are defined, with message texts kept outside the components and the e-mails the app sends | The UI itself |
| Project rules | Which of the project's rules bind the pages | Agent instructions and docs that bind user-facing text | None |

Scope names the areas the guide covers and the areas it leaves out, each with its reason.
List the app's areas, such as public pages, signed-in sections and administration. Give an
out reason as the fact that keeps the area out, so a change that removes the fact reopens
the question. The options for Scope are ways to draw the line: the proposed set of areas,
then that set with a debatable area moved in or out. With nothing in scope, save the
settings and stop: the guide stays empty until the scope changes.

Delivery is **built** when readers can reach the pages the way the setting names; a guide
read in the repository is built from the start. Until then it is **pending**. The options
for Delivery are an in-app route and the repository alone. A help route the app already
has is the in-app option as it stands, recorded as built. Otherwise, to word the in-app
option, take the route's path and the place of its link from the Route and Navigation link
rules in [`in-app-delivery.md`](in-app-delivery.md). Setup records that delivery as
pending and builds nothing; the guide plan has the route built later.

With **generated** screenshots, a browser spec or script captures the named screens from
fixture data into the guide's image folder, and one command regenerates all of them. A
browser suite in the project makes pages with and without screenshots two candidates.

## Settings file

Save every setting as the user left it, whether picked, corrected or given in their own
words, to `.agents/user-guide.md`:

```markdown
# User guide settings

- Language: <language of the pages, and the form of address>
- Location: <folder>, index at `<folder>/index.md`
- Scope: in: <area, with its routes or paths>; out: <area>: <reason>
- Audiences: <role>: <what they do in the app>; <role>: <…>
- Viewport: <device or device class, with width × height>
- Walk setup: <how to start the app; how to sign in as each audience; where sign-in messages are read locally>
- Delivery: <in-app route `/<path>`, link in <navigation element> | repository only | …>, <built | pending>
- Step sources: <paths to browser specs; where QA tickets live and how to recognise them>
- Screenshots: <none | generated by `<command>` into `<folder>`>
- Terms: <where the app's own words for its concepts are found: components, message texts outside them, e-mails>
- Project rules: <rules pages must respect, with their source>
- Plan: <none | ticket link>
```

`Plan` starts as none and is not put to the user.

## Pointer in the agent instructions

Propose one line for the project's agent instructions file, in its delivery or completion
section, so every implementation session meets it:

```markdown
A change to what users see or do carries its user-guide update, or its entry in the guide plan, in the same pull request; settings are in `.agents/user-guide.md`.
```

## After setup

Done when: every question has the user's answer, the shown settings went to the user in
the same message and carry their corrections, the settings file holds a value for every
setting, delivery is built or recorded as pending, and the pointer line is added or the
user declined it.

With something in scope, go back to the routes in [`SKILL.md`](SKILL.md) and take the
first that now fits.
