# syafiqkassim.github.io

Personal academic website of Syafiq Kassim, Gtech. (PhD candidate, School of Computing, UUM; Assistant IT Officer, UUMIT; research on QR code phishing / cybersecurity / explainable AI).

- Live: https://syafiqkassim.github.io (GitHub Pages, served from `main`, no build step)
- The owner usually edits through Claude Code on the web. **Reply to him in Malay**; the website content itself is in **English**.

## Structure

Everything is in a single file, `index.html`: inline CSS, inline JS, no framework, no build. Do not add a build tool, package.json or extra frameworks.

Sections (in order, each `<section class="block" id="...">`): `about`, `work`, `research`, `writing`, `contact`. The hero has a "ledger" of credentials (Study / Role / Registration).

## Adding or editing an article

Articles live as JSON in `<script type="application/json" id="articles-data">[ ... ]</script>`. Add a new object to that array; the JS renders the list (newest first) and the reader page (`#a-<slug>`).

```json
{
  "slug": "lowercase-words-with-hyphens",
  "title": "Article title",
  "date": "YYYY-MM-DD",
  "tags": ["QR phishing", "Cybersecurity"],
  "summary": "One or two sentences shown in the article list.",
  "body": "Paragraph text...\n\n## Heading\n\n- list item\n- list item\n\nCite sources as [1], [2].",
  "refs": ["Author. (Year). Title. Publisher. https://..."]
}
```

Body format (mini-markdown, parsed by `renderBody`):
- Blank line (`\n\n`) between blocks.
- `## ` at the start of a block = heading. A block where every line starts with `- ` = bullet list.
- `[n]` = citation, matching the n-th entry in `refs` (1-based). Only `https://` links in refs become clickable.
- No HTML, bold or italics in the body; it is rendered as text.

Rules:
- Keep the JSON valid. Escape `<` as `<` inside the JSON (a literal `</script>` would break the page).
- `slug` must be unique; it becomes the URL `#a-<slug>`.
- Every factual claim or statistic needs a real, verifiable reference. Never invent sources, figures or DOIs; ask the owner if a source is missing.
- Writing style: clear, for a general audience, British/Malaysian English spelling.

## Design

Colour and font tokens are CSS variables on `:root` (light) with a dark-mode override under `prefers-color-scheme: dark` and `[data-theme="dark"]`. When changing colours, update **both** light and dark values. Fonts: Bricolage Grotesque (display), Instrument Sans (body), JetBrains Mono (labels). Keep it responsive (check at ~375px width; no horizontal scroll).

## Leftover editor code

The JS contains an owner-only "New article" editor that only works inside a Claude artifact (`window.claude`). On GitHub Pages it stays hidden and does nothing. The old Claude artifact copy of this site is **no longer the source of truth**: this repo is. Don't remove that code unless asked.

## Publishing

1. Make the change in `index.html`.
2. Check the JSON parses and the page has no obvious breakage.
3. Commit with a short clear message and push (or open a PR if working on a branch). GitHub Pages updates in 1–5 minutes.
