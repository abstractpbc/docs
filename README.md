# Abstract docs

The public developer documentation for [Abstract](https://abstract.inc): the robot action
models and the LLM models behind `https://api.abstract.inc/v1`. It is a
[Mintlify](https://mintlify.com) site.

## Source of truth

The source lives in the Abstract monorepo at `apps/docs`. Edit it there. A copy in any other
repository is a publish target, and an edit made only in the copy is lost on the next publish.

Two tests in the monorepo fail when these pages drift from the code that serves the API:

- `apps/web/src/docs.test.ts` (in `bun run check`): every page in `docs.json` exists and every
  page is listed; the documented model ids equal the served ones; no retired or unserved id,
  internal host, or secret name appears; the OpenAPI file names only served models.
- `apps/engine/tests/test_docs.py` (in the engine CI job): each table on
  `robots/models.mdx` matches the model's embodiment in `apps/engine/models`, each documented
  `robot_state` is accepted, and every robot Python example runs against the engine service
  on a CPU, with no model weights and no network.

## Publish

`apps/docs/publish.sh`, run from a clean checkout of a pushed monorepo commit, copies this
directory (without the script) to the public repository
[abstractpbc/docs](https://github.com/abstractpbc/docs). Mintlify deploys
https://docs.abstract.inc from every push to that repository's `main` branch.

## Preview

Install the Mintlify CLI (Node.js 20.17 or later, an LTS release recommended) and run it in
this directory:

```sh
npm i -g mint
mint dev
```

The site opens at http://localhost:3000.

```sh
mint validate      # strict build, including the OpenAPI file
mint broken-links  # internal links
```

## Layout

| Path | Content |
| --- | --- |
| `docs.json` | Theme, colours, fonts, navigation, and API reference settings |
| `*.mdx`, `robots/`, `llm/` | Guide pages |
| `api-reference/openapi.yaml` | The OpenAPI 3.1 description the API reference tab is generated from |
| `logo/`, `favicon.png` | The Abstract mark, built from the site's brand mark |
| `fonts/` | Geist (SIL Open Font License 1.1, see `fonts/OFL.txt`) |

The API reference shows each endpoint without a playground (`api.playground.display` is
`simple`), so the docs never send a request with a reader's key.
