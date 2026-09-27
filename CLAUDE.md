# privacy-policies

## Project Overview
Static HTML privacy policy pages for the ClickTo Windows utilities — one self-contained
`index.html` per app under `<App>/privacy-policy/` (currently `ClickToPDF` and `ClickToSlideshow`).

## Build
No build step — plain static HTML/CSS files.

Duplication check:
```
npx jscpd --reporters console .
```

## Tests
No tests.

## Architecture
See `docs/CURRENT_APPLICATION.md` for full detail.

- `ClickToPDF/privacy-policy/index.html` — privacy policy for ClickTo: PDF
- `ClickToSlideshow/privacy-policy/index.html` — privacy policy for ClickTo: Slideshow

Each page is self-contained (inline CSS, light/dark via `prefers-color-scheme`, no external assets).

## Key Conventions
- Pages are standalone: inline styles, no shared stylesheet, no JavaScript.
- Keep the pages visually consistent with each other when editing one.

## Code standards

These are generation-time rules. They are deliberately short because they cost context on
every turn. The full review criteria live in the `slop-review` agent and are only paid for
when a review runs.

**Search before you write.** Grep for an existing helper, type, constant, or utility that
already does this before adding a new one. Adding a near-duplicate is a defect, not a
neutral choice. If you looked and found nothing, that is worth one line in your summary.

**Name things by what they hold or do, not what they are about.**
- A name says what it returns or what it changes: `activeUserEmails`, not `userData`.
- Never a category noun on its own: Manager, Handler, Helper, Util, Service, Processor,
  Info, Wrapper, Data, Item.
- Never a name that lies: no `get*` that mutates or does I/O, no `validate*` that also
  saves, no `is*`/`has*` returning a non-boolean, no `*All` that silently paginates.
- Don't stutter against the container: `Users.create` not `UserService.createUser`;
  `user.name` not `user.userName`.
- The surrounding file's existing vocabulary beats every rule above. If this module says
  `client`, don't introduce `customer` for the same thing.

**Split functions on abstraction level, not line count.**
- A function should sit at one altitude: orchestration, or domain logic, or I/O — not all
  three in one body.
- If you can't name it without "and", or the best name is handle/process/manage/do + noun,
  it is doing two things.
- A boolean parameter that switches behaviour is two functions.
- **And the opposite failure, which is just as bad:** do not extract a block that would need
  four or more parameters or return three or more values — that is a cut across the grain,
  not a seam. Do not create a helper called from exactly one place whose name describes its
  position in the caller (`step2`, `handleRest`, `_inner`, `processData2`). One sixty-line
  function at a single altitude beats six ten-line functions that only make sense read in
  call order.

**No speculative structure.** No interface or abstract base with one implementation, no
config option nothing sets, no extensibility hook for a second case that does not exist.
Equally: don't inline something used in three places to avoid "over-abstracting".

**Errors: catch only what you can act on.** No try/catch around code that cannot throw. No
broad catch that logs and continues with now-invalid state. Never swallow an exception to
make a test pass.

**Comments say why, not what.** Delete any comment that restates the line below it. Keep
comments that record a constraint, a rejected alternative, or a non-obvious reason.

**Tests must be able to fail.** For every test you write, you should be able to name the
specific bug it catches. Do not mock the unit under test. Do not assert only that something
is non-null, non-empty, or did not throw. Do not derive the expected value by running the
code and pasting the output.

**Dependencies.** Don't add one for something under about twenty lines of obvious code.
Don't reimplement what the standard library already does correctly.

**Say what you didn't do.** When you finish a unit of work, state plainly anything you
guessed at, stubbed, skipped, or could not verify. An unflagged guess is worse than an
admitted gap.

**Review cadence.** Run `/checkpoint` after each unit of work, and `/slop-check` before a PR
or at the end of a feature. Both are available from user scope — nothing to install here.

## Notes
- Public repo `The-Bureau-of-Stuff/privacy-policies`, served by GitHub Pages from `main` root.
  Exists so the app repos can stay private (the org is on the Free plan, which cannot serve
  Pages from private repos).
- Page URL pattern: `https://the-bureau-of-stuff.github.io/privacy-policies/<App>/privacy-policy/`.
  These URLs are submitted to Partner Center — never rename a folder once published.
