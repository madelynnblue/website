# Website

Personal portfolio/blog built with [Eleventy](https://www.11ty.dev/) (v1.0.0).

## Commands

- `npm start` — dev server with live reload
- `npm run build` — production build (sets `NODE_ENV=production`)

Output goes to `build/` (gitignored).

## Content

Posts live under `src/posts/` in topic subdirectories:

- `organ/` — music arrangements and organ articles
- `programming/` — technical writing (Go, SQL, CockroachDB)
- `projects/` — project showcases
- `various/` — miscellaneous

Each directory has a `.json` file that sets frontmatter defaults for all markdown in that directory (section, subsection, layout).

### Post frontmatter fields

- `title`, `date` (YYYY-MM-DD), `section`, `subsection` — required for categorization
- `article` — display name in section listings (falls back to `title`)
- `lilypond`, `opus`, `youtube`, `chiptune` — optional media embeds rendered by `post.njk`

The homepage auto-generates navigation from the `sections` collection, which groups posts by `section`/`subsection` and sorts by `article`.

## Layouts

All templates live in `src/_includes/layouts/`:

- `base.njk` — HTML shell, fonts, all styles
- `home.njk` — homepage with auto-generated section nav
- `post.njk` — article layout with optional media blocks
- `simple.njk` — minimal layout (used by `/me`)

## Static assets

Everything under `public/` is copied as-is to the build output. Audio files are `.opus` format in `public/opus/`.

## Style

Prettier with 2-space indentation. `.prettierrc` treats `.njk` files as HTML.
