# codel-content

The pattern library behind [codel.app](https://codel.app) — a copy-paste
library of AI coding patterns (loops, goals, workflows, prompts) for vibe
coders, covering engineering, debugging, SEO, market research, marketing, and
more.

This repo IS the database. codel.app fetches this repo at build time and
regenerates the site. There is no other content store.

## Structure

```
patterns/
  <category>/
    <slug>.md
skill/
  SKILL.md            # installable agent skill for consuming codel patterns
schema/
  pattern-schema.md   # the frontmatter contract + validation rules
```

## Contributing a pattern

The easiest way to contribute is the [submission form on codel.app](https://codel.app/submit) —
fill it in, and if approved it lands here automatically with your
attribution.

To contribute directly via pull request:

1. Read `schema/pattern-schema.md` for the exact frontmatter contract.
2. Copy the template from that doc into a new file at
   `patterns/<category>/<your-slug>.md`.
3. Make sure the pattern block in `## The pattern` is self-contained: pasting
   only that block into an AI tool should work with zero other context.
4. Open a PR. `codel-site`'s build validates every file and will fail loudly
   on a broken or duplicate-slug pattern.

## License

- Pattern content (`patterns/`): [CC BY 4.0](LICENSE-CONTENT.md) — attribute
  the author listed in each pattern's frontmatter.
- Code (e.g. `skill/`): [MIT](LICENSE).

## Links

- Site: https://codel.app
- For AI agents: https://codel.app/for-agents
- Static API: https://codel.app/api/patterns.json
