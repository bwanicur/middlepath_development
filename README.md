# Middle Path Development — Site

The marketing site for Middle Path Development, a software consultancy in San Diego, California.
Built with [Astro](https://astro.build) as a static, three-page site: no CMS, no framework
components, no client-side router. Content lives in the pages themselves.

## Pages

| Route       | File                     | Purpose                                                                                 |
| :---------- | :----------------------- | :-------------------------------------------------------------------------------------- |
| `/`         | `src/pages/index.astro`  | Hero, the three offerings (Project Rescue, Greenfield Builds, Consultancy), how we work, CTA band |
| `/projects` | `src/pages/projects.astro` | Longer description of each kind of engagement, with the specifics of what it includes   |
| `/people`   | `src/pages/people.astro` | Founder bio for Ben Wanicur and the working tech stack                                   |

Page content is defined as plain arrays in each page's frontmatter (`offerings`, `principles`,
`projectTypes`, `stack`) and rendered inline — edit the array to change the copy.

## Project Structure

```text
/
├── public/
│   └── favicon.svg
├── src/
│   ├── assets/
│   │   └── middlepath-logo.png   # optimized via astro:assets <Image>
│   ├── components/
│   │   ├── ContactToggle.astro   # click-to-reveal email address
│   │   └── PathDivider.astro     # winding-path SVG section divider
│   ├── layouts/
│   │   └── Layout.astro          # <head>, fonts, header nav, footer
│   ├── pages/
│   │   ├── index.astro
│   │   ├── people.astro
│   │   └── projects.astro
│   └── styles/
│       └── global.css            # design tokens, base styles, shared utilities
├── astro.config.mjs
└── package.json
```

## Components

**`Layout.astro`** — the only layout. Takes `title` and `description` props, which drive both the
document title/meta description and the Open Graph tags. It imports the fonts and `global.css`,
and renders the header (logo, nav, contact toggle) and footer. Nav links are a single `navLinks`
array reused by both, with `aria-current="page"` set from `Astro.url.pathname`.

**`ContactToggle.astro`** — a button that reveals the contact email on click. The address is never
present in the static HTML: it ships as reversed string fragments and is reassembled in the browser,
so scrapers find nothing matching an email pattern. Four style variants via the `variant` prop —
`nav` (popover under the header), `solid` and `invert` (button, reveals below), and `inline`
(default; sits inside a sentence). Escape closes any open panel.

**`PathDivider.astro`** — the site's signature motif, a winding SVG path between sections, echoing
the river in the logo. Purely decorative (`aria-hidden`), inherits `currentColor`.

## Styling

All styling is plain CSS — no Tailwind, no preprocessor. `src/styles/global.css` defines the design
tokens and shared classes; per-component tweaks live in scoped `<style>` blocks inside each
`.astro` file.

The palette is derived from the logo — pine and meadow greens for the hillsides, a river blue for
the path — exposed as custom properties (`--pine`, `--meadow`, `--river`, `--fog`, `--ink`, …).
Type is Zilla Slab for display, Public Sans for body, IBM Plex Mono for the revealed email address,
all self-hosted through Fontsource. The `blaze-river` / `blaze-meadow` / `blaze-pine` classes are
the trail-blaze marks that color-code the three offerings consistently across pages.

## Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build the production site to `./dist/`           |
| `npm run preview`         | Preview the build locally, before deploying      |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

Requires Node.js 22.12 or newer. The build output in `./dist/` is fully static and can be deployed
to any static host.
