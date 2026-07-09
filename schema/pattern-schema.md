# Pattern file contract

Every pattern is a single markdown file with YAML frontmatter, stored at
`patterns/<category>/<slug>.md`.

## Frontmatter fields

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `title` | string | yes | Human title. |
| `slug` | string | yes | Kebab-case, unique across the entire catalog, must match the filename (without `.md`). |
| `format` | string | yes | One of: `loop`, `goal`, `workflow`, `prompt`. |
| `category` | string | yes | One of the 10 category slugs below. Must match the folder the file lives in. |
| `tools` | string[] | yes | Any of: `claude-code`, `cursor`, `codex-cli`, `gemini-cli`, `universal`. At least one entry. |
| `difficulty` | string | yes | One of: `beginner`, `intermediate`, `advanced`. |
| `est_time` | string | yes | Free text, e.g. `"10 min"`. |
| `models` | string[] | yes | Any of: `claude`, `gpt`, `gemini`, `any`. At least one entry. |
| `summary` | string | yes | Max 120 characters. Shown on cards and in the API. |
| `author` | string | yes | `"codel"` for house content, or the contributor's name. |
| `author_handle` | string | no | Contributor's X handle, without `@`. |
| `date` | string | yes | ISO date `YYYY-MM-DD`. |
| `license` | string | yes | `"CC-BY-4.0"`. |

## Category slugs (exactly 10)

`engineering-coding`, `debugging-evaluation`, `operations-devops`,
`product-planning`, `content-writing`, `design-ui`, `seo-geo`,
`market-research`, `competitive-intelligence`, `marketing-growth`.

## Body sections (required, in order)

1. `## When to use this` — 2-3 lines, a specific situation.
2. `## The pattern` — a fenced ` ```text ` block. This is the only thing the
   site's copy button copies. Must be self-contained: pasting only this block
   into an AI tool must work with no other context, with zero editing inside
   the prose. No `[BRACKET]` placeholders. If the agent can discover an input
   itself (the codebase, the default branch, the framework), tell it to. If
   real user input is needed, defer it: reference "the URL I paste below" and
   end the block with at most 3 labeled fill-in lines (e.g. `My site URL:`)
   or a paste marker (`Paste the draft below this line:`).
3. `## Real example output` — a short, real result.
4. `## Why it works` — 2-3 lines on the mechanism.

## Validation rules (enforced by `codel-site/scripts/build-index.ts`)

The site's build fails loudly if any pattern file:

- is missing any required frontmatter field
- has a `slug` that does not match its filename
- has a `slug` that duplicates another pattern's slug anywhere in the catalog
- has a `category` that does not match its folder name
- has a `format`, `category`, `difficulty`, `tools`, or `models` value outside
  the allowed sets above
- is missing the `## The pattern` section or its ` ```text ` code block
- has a `summary` longer than 120 characters

This keeps the public repo honest even when pull requests arrive from the
community.
