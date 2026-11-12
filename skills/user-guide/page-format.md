# Page format

The reader arrives mid-task, on the device the app is used on, wanting the next action.

Use the words the app shows, taken from the terms source in the settings; where the code
and the UI name a thing differently, the UI wins. Address the reader directly. Examples
use invented data. Every page keeps the project rules in the settings. Describe the app as
it works today, so a page reads the same to someone who never saw an earlier version.

Draw the steps from the step sources named in the settings and from the change itself. QA
scenarios belong to `manual-qa`: a QA ticket linked to the change supplies ordered steps,
starting state and expected results.

The templates below name their sections in English. Translate the headings into the guide
language once: the guide's first page fixes them, and every later page copies them.

## How-to page

One page per goal.

- **File:** `<location>/<goal-slug>.md`. The slug is the goal in the guide language,
  lower-case ASCII with hyphens.
- **Title:** the goal as the user would ask for it.
- **Before you start:** who can do this and what must already be true. Leave the section
  out when every signed-in user can do it from anywhere.
- **Steps:** numbered, one action each, starting from a place the reader can find, such as
  a navigation item. Quote every control and message in bold exactly as the app shows it.
  Say what appears after an action where that confirms the reader is on track. A step
  only one audience takes opens by naming that audience; an optional step says it can be
  skipped. Where the steps pass through a run that has its own how-to page, link to that
  page in the step where the run begins and continue with the step after it. Replace the
  part of a quoted message that varies, such as a number or a name, with `…`.
- **Consequences:** a limit or a consequence that is hard to undo goes in the step that
  triggers it, before the action. When a concept page holds the rule behind it, state the
  consequence in one clause and link to that page.
- **Result:** what the reader sees when the goal is reached, and what changes for other
  people when that is the point of the goal.
- **If something else happens:** each other state this flow can end in, apart from field
  validation that names the field to correct. Quote its message, then give the next
  action.
- **Related:** the page for the goal that usually comes next. Leave the section out until
  that page exists.

The page of a run that other goals pass through is a how-to page with these differences:
the title names the action the run performs, the steps go from the screen where the run
begins to the screen where it hands back, and the result says where the reader carries on.
It closes with a **Used in** section linking to every how-to page that passes through the
run, added when the first such page exists; each later one adds its own link.

With generated screenshots, add an image to a step whose control is hard to locate, and
add that screen to the capture so the image regenerates with the rest.

```markdown
# <Goal as the user would ask for it>

## Before you start

- You are <role>.
- <What must already be true>.

## Steps

1. Open **<navigation item>**.
2. Select **<control label>**. <What appears>.
3. <Consequence that is hard to undo>, see [<concept>](<concept-slug>.md). Select **<control label>**.

## Result

<What the reader sees, and what changes for others.>

## If something else happens

- **<Message as shown>:** <next action>.

## Related

- [<Next goal>](<next-goal-slug>.md)
```

## Concept page

One page per rule, and the only place the guide states that rule in full.

- **File:** `<location>/<concept-slug>.md`. The slug is the concept's name in the app, formed
  like a how-to slug.
- **Title:** the concept as the app names it.
- **The rule:** the opening one or two sentences state it outright.
- **What you see:** where the rule shows in the app, with labels and states quoted in bold.
- **What it means for you:** what the reader does differently because of it.
- **Used in:** links to every how-to page that relies on the rule. A how-to page that
  starts relying on it adds its own link here.

A concept page has no numbered steps: an action the reader takes belongs on a how-to page.

```markdown
# <Concept as the app names it>

<The rule, in one or two sentences.>

## What you see

<Where the rule shows in the app: **<label>**, **<state>**.>

## What it means for you

<What the reader does differently because of it.>

## Used in

- [<Goal>](<goal-slug>.md)
```

## Index

`index.md` opens the guide: one line on what the app is for, then the how-to pages grouped
by audience, in the order a new user meets them, then the concept pages in one group. A
page several audiences use sits in one group named for those audiences, ahead of the
single-audience groups. Each item is a link titled with its goal or concept. Every page is
linked from the index exactly once.
