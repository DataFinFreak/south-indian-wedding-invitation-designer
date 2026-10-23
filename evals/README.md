# Evals

`evals.json` holds five test scenarios for the skill. Each has a `prompt` (what a user types) and `expected_behaviors` (what a good run does). All names, dates and venues are fictional.

| # | Scenario | What it tests |
|---|---|---|
| 1 | Kannada couple, digital invite | The full workflow: lock facts, three directions, config filling, regional event names kept, QA, share message. |
| 2 | Tamil traditional | Family-hosted order, regional-language line only when supplied or approved, VERIFY flags, invocation opt-in. |
| 3 | Minimalist English | Contemporary preset, optional sections hidden, contrast, no added cultural or religious content. |
| 4 | Add our photo | Consent, cut-out recipe, measured rotation, face-centred crop, metadata stripping, honest retouching. |
| 5 | Incomplete details | Must ask, never invent; visible placeholders and the `?check` view. |

## How to run

1. Install the skill in the environment you want to test (claude.ai upload, Claude Code plugin, or a skills folder).
2. For each scenario, start a fresh conversation and paste the prompt. Answer follow-up questions consistently with the prompt; when the prompt does not cover a question, accept the assistant's recommended answer.
3. Score each expected behaviour as pass or fail. A run passes when every behaviour passes; behaviours about honesty (never inventing details, VERIFY flags, no silent translation) are blocking.
4. Repeat the same prompt without the skill to see what the skill changes.
5. Test across model sizes where possible.

For scenario 4, use only a photo you have consent to use, or a non-identifying sample portrait. Do not commit test photos to the repository.

The `files` field lists any inputs the tester must supply. Results are not stored in the repository.
