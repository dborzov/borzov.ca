# AGENTS.md

Source for the personal website www.borzov.ca, built with Hugo. The built site is deployed from a second repo cloned into `public/`; see the deploy file below.

## Rules

- Both this repo and the deploy repo are public on GitHub. Never commit anything private: local file paths, personal details that are not already on the site, credentials, or dev-server output.
- A post's URL comes from its file name: `content/posts/<name>.md` is served at `/posts/<name>/`. Renaming a post breaks existing links. Keep the `date` in the front matter when reworking a post.
- Every file in `content/posts/` with `draft = false` gets published, even if nothing links to it. Do not leave copies of a post there.
- The homepage list of posts is written by hand in `content/home/home.md`, under "Words in specific order". A new post needs an entry there. That file has CRLF line endings; keep them.
- Raw HTML in Markdown is stripped when the site is built. Use a shortcode from `layouts/shortcodes/` instead, for example `{{< tldr >}} … {{< /tldr >}}` for the summary box at the top of a post.
- Tables of contents in posts are written by hand. After renaming a heading, update the links to it and check that every `#anchor` matches a heading id in the built page.

## Commands

- `hugo server --renderToMemory`: local preview at http://localhost:1313/. Without `--renderToMemory`, current Hugo writes the preview into `public/`, with `localhost` links and a live-reload script that must never be deployed.

## Reference files

Read a file only when its trigger applies.

- [docs/deploy.md](docs/deploy.md): read before building for production, deploying, or committing anything in `public/`. Holds the two-repo setup and the deploy steps with their catches.

## Maintaining these docs

This file holds only what every task needs. Details go in a focused file in `docs/`, linked directly from this file. Do not describe the architecture or the file layout here. When a task teaches something a later agent would get wrong, add it to the right file and mention the edit in your final report.
