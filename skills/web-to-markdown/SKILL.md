---
name: web-to-markdown
description: "Use when the user wants to scrape a webpage, convert a URL to markdown, fetch website content as text, or extract page text for reading. Converts webpage URLs to clean Markdown by calling the local web2md CLI (Puppeteer + Readability), handling JS-rendered pages, login walls, and batch URL conversion."
metadata:
  version: 0.1.0
---

# web-to-markdown

Convert web pages to clean Markdown using the `web2md` CLI. Handles JS-rendered pages via Puppeteer, extracts main content with Readability, and outputs clean Markdown with Turndown.

## Non-goals

- Do not use Playwright or other browser automation stacks; always use `web2md`.

## Inputs (ask only if missing)

- `url` (or a list of URLs)
- Output preference: print to stdout (`--print`), save to file (`--out ./file.md`), or save to directory (`--out ./dir/`)

## Workflow

1. Validate URL(s) start with `http://` or `https://`.
2. Ensure `web2md` is installed:
   - Run: `command -v web2md`
   - If missing, instruct the user to install from `~/workspace/softaworks/projects/web2md`:
     ```bash
     cd ~/workspace/softaworks/projects/web2md && npm install && npm run build && npm link
     ```
3. Convert using the appropriate pattern:
   - **Single URL to file:** `web2md '<url>' --out ./page.md`
   - **Auto-named in directory:** `mkdir -p ./out && web2md '<url>' --out ./out/`
   - **Login walls / human verification:** `web2md '<url>' --interactive --user-data-dir ./tmp/web2md-profile --out ./out/`
   - **Print to stdout:** `web2md '<url>' --print`
   - **Batch (multiple URLs):** Create output dir, then run one `web2md` command per URL with `--out ./out/`
4. Validate output: verify files exist and are non-empty (`ls -la <path>` and `wc -c <path>`).
5. Return the saved file path(s), or the Markdown content (stdout mode).

## Rendering controls (for tricky pages)

Use these flags when pages don't render correctly with defaults:

| Flag | Purpose |
|------|---------|
| `--wait-until networkidle2` | Default; wait for network to settle |
| `--wait-until domcontentloaded --wait-ms 2000` | Heavy apps; add `--wait-for 'main'` if needed |
| `--chrome-path <path>` | Override Chrome auto-detection |
| `--interactive` | Pause for human login/captcha completion |
| `--wait-for '<selector>'` | Wait for a specific CSS selector |
| `--headful` | Show browser window for debugging |
| `--no-sandbox` | Required in some containers/CI environments |
| `--user-data-dir <dir>` | Persist login sessions across runs |
