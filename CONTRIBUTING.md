# Contributing

Improvements are welcome: new style presets, regional wording checked by native speakers, bug fixes in the template, better docs, and test results from real phones.

## How

1. Fork the repo and create a branch.
2. Make your change. Keep `SKILL.md` short; put detail in `references/`.
3. Check it:
   - the template opens without console errors at 360–430 px wide and has no horizontal scroll;
   - text passes WCAG AA contrast (4.5:1 body, 3:1 large);
   - `npx skills-ref validate skills/south-indian-wedding-invitation-designer` passes.
4. Open a pull request saying what you changed and how you tested it (screenshots help).

## Rules

- **No real personal data.** Use clearly fictional names, dates and venues, or the `[PLACEHOLDER]` tokens. No photos of real people.
- **Only assets you have the right to share.** Original artwork, or assets whose licence allows redistribution; list any third-party asset in `THIRD_PARTY_NOTICES.md`. Do not add music.
- **Regional wording:** say which language and who checked it. Unchecked wording stays marked VERIFY.

By submitting a pull request you agree your contribution is licensed under the repository's Apache-2.0 licence.
