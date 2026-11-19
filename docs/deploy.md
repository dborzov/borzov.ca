# Deploying www.borzov.ca

How the site is hosted and how to deploy a change. Read before building for production, deploying, or committing anything in `public/`.

## Setup

- This repo (`dborzov/borzov.ca`) holds the Hugo source.
- The built site lives in a second repo, `dborzov/dborzov.github.io`, served by GitHub Pages at www.borzov.ca (its `CNAME` file sets the domain).
- That repo is cloned into `public/`, Hugo's output directory. This repo tracks `public` only as a pointer to a commit of the deploy repo, with no `.gitmodules`, so a fresh clone of this repo leaves `public/` empty.
- The deploy repo also holds files Hugo does not generate: `CNAME`, `404.html`, `keybase.txt`, `sores/`, `.forestryio`. Never delete them.

## Deploy steps

1. Commit and push the source change in this repo.
2. Get an up-to-date clone in `public/`:
   - If `public/` is empty: `git clone git@github.com:dborzov/dborzov.github.io.git public`.
   - If `public/` has files but no `.git`, they are leftover preview output. Move them out of the way, then clone.
   - Otherwise: `git -C public pull`.
3. Build with a plain `hugo` from the repo root. Do not pass `--cleanDestinationDir`: it would delete the files Hugo does not generate.
4. Hugo never deletes output. When a page or a file in `static/` was removed from the source, `git rm` its built copy in `public/` by hand, after checking with `grep -r` that nothing else in `public/` links to it.
5. Review before committing. Read `git -C public status` and the diff, and make sure:
   - `git -C public diff | grep -iE 'localhost|127\.0\.0\.1|livereload'` prints nothing (a match means preview output got in);
   - `CNAME` and the other files above are untouched;
   - nothing private is in the diff.
6. Commit and push in `public/`. GitHub Pages publishes within about a minute; check the changed pages on https://www.borzov.ca/.
7. Record the deployed commit in this repo: `git add public`, then commit ("Update public to the deployed build") and push.

## Expected noise in the diff

The live site was once built with a much older Hugo, so a rebuild can change the HTML of posts that were not edited (code highlighting markup, heading ids) and the RSS feeds. That is fine, provided the visible text is unchanged.
