# syafiqkassim.github.io

Personal academic website of Syafiq Kassim, Gtech. (PhD candidate, School of Computing, UUM; Assistant IT Officer, UUMIT; research on QR code phishing / cybersecurity / explainable AI).

- Live: https://syafiqkassim.github.io (GitHub Pages, served from `main`, no build step)
- The owner usually edits through Claude Code on the web. **Reply to him in Malay**; the website content itself is in **English only**. The owner decided (Nov 2026) not to make the site bilingual: don't add a BM version or language switcher. Official Malay programme names (e.g. in Training) stay as they are.

## Structure

Almost everything is in `index.html`: inline CSS, inline JS, no framework, no build. Articles are in `data/articles.json`; the CMS lives in `admin/`. Do not add a build tool, package.json or extra frameworks.

Sections (in order, each `<section class="block" id="...">`): `about`, `work`, `research`, `training`, `csr`, `writing`, `contact`. The `csr` section is filled from `data/csr.json` (array of `{organisation, role, description?}`, shown in file order, hidden when empty; editable in the CMS). The `training` section is filled from `data/training.json` (array of `{title, start: "YYYY-MM-DD", end?}`; multi-day courses use one entry with `end`), grouped by year with the newest year open; it is also editable in the CMS. The hero has a "ledger" of credentials (Study / Role / Registration).

## Adding or editing an article

Articles live in **`data/articles.json`**: a JSON array, one object per article. `index.html` fetches it at load time and renders the list (newest first) and the reader page (`#a-<slug>`). The inline `<script id="articles-data">` in `index.html` is just an empty `[]` fallback; don't put articles there.

The owner can also edit articles himself through **Sveltia CMS** at https://syafiqkassim.github.io/admin/ (`admin/index.html` + `admin/config.yml`; sign in with a GitHub token). The CMS commits straight to `main` and may reformat the JSON. So **always `git pull` before editing**, and keep the field names below in sync with `admin/config.yml`.

```json
{
  "slug": "lowercase-words-with-hyphens",
  "title": "Article title",
  "date": "YYYY-MM-DD",
  "tags": ["QR phishing", "Cybersecurity"],
  "image": "/images/articles/file-name.webp",
  "image_whole": false,
  "image_alt": "Short description of the picture",
  "image_caption": "Photo: source or credit",
  "summary": "One or two sentences shown in the article list.",
  "body": "Paragraph text...\n\n## Heading\n\n- list item\n- list item\n\nCite sources as [1], [2].",
  "refs": ["Author. (Year). Title. Publisher. https://..."]
}
```

`image`, `image_alt` and `image_caption` are optional. Images are stored in `images/articles/` (the CMS uploads there and converts to WebP, max 1600px). The site only shows `image` values starting with `/images/` or `https://`, always cropped to 16:9 (`.cover`): a 13rem thumbnail beside the title in the list (above it under 560px) and full width at the top of the article. Recommended upload: 1600 × 900 px landscape. `image_whole: true` adds class `whole` (object-fit: contain on a plain background) for square images, diagrams and charts.

Body format (mini-markdown, parsed by `renderBody`):
- Blank line (`\n\n`) between blocks.
- `## ` at the start of a block = heading. A block where every line starts with `- ` = bullet list.
- `[n]` = citation, matching the n-th entry in `refs` (1-based). Only `https://` links in refs become clickable.
- No HTML, bold or italics in the body; it is rendered as text.

Rules:
- Keep the JSON valid (check with `python3 -c "import json;json.load(open('data/articles.json'))"`).
- `slug` must be unique; it becomes the URL `#a-<slug>`.
- Every factual claim or statistic needs a real, verifiable reference. Never invent sources, figures or DOIs; ask the owner if a source is missing.
- Writing style: clear, for a general audience, British/Malaysian English spelling.

## Design

"Security console" look aimed at cybersecurity employers (redesigned Nov 2026; the owner plans to apply for UK jobs in 2027). Colour tokens are CSS variables on `:root` (light) with dark overrides under `prefers-color-scheme: dark` and `[data-theme="dark"]`; update **both** when changing colours. A header toggle saves the theme in `localStorage`. Fonts: Space Grotesk (display), IBM Plex Sans (body), JetBrains Mono (labels/terminal). Keep it responsive (check at ~375px width; no horizontal scroll) and respect `prefers-reduced-motion`.

Interactive parts live in `<script id="ui-script">` (after `site-script`):
- Hero terminal: commands such as `help`, `whoami`, `skills`, `training`, `csr`, `writing`, `contact`, `sudo hire`. Text is in the `commands` object; keep it factual.
- Typing role line (`roles` array), stats strip (counted from the data files via the `site:data` event / `window.SITE`), scrollspy, scroll reveal, mobile menu.
- "Quish or legit?" lab in Research: `rounds` array of **fictional** scenarios and domains. Don't use real organisations' domains as phishing examples.
- Training filter: tags are assigned from title keywords (`T_TAGS` in `site-script`); add keywords there if a new programme ends up untagged.
- "Platforms and technologies I have been trained on" chips in Work are hard-coded and derived from `data/training.json`; only list things he actually trained on.

## Leftover editor code

The JS contains an owner-only "New article" editor that only works inside a Claude artifact (`window.claude`). On GitHub Pages it stays hidden and does nothing. The old Claude artifact copy of this site is **no longer the source of truth**: this repo is. Don't remove that code unless asked.

## Publishing

1. `git pull` (the CMS may have pushed changes), then make the change in `index.html` or `data/articles.json`.
2. Check the JSON parses and the page has no obvious breakage.
3. Commit with a short clear message and push (or open a PR if working on a branch). GitHub Pages updates in 1–5 minutes.
