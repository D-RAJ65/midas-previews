# midas-previews — Claude Code Instructions

## What This Repo Is

This is a GitHub Pages deployment repo that serves shareable HTML slide-deck previews at `https://preview.midasdecks.com`.

It is **not a source code project**. There is no build step, no package.json, no dependencies. It is a flat collection of self-contained HTML files that are committed and pushed by the `deckify.py` tool in the sister repo `/home/dmahtani/Coding/deckify/`.

## Tech Stack

- Static HTML files — each file is a fully self-contained interactive slide viewer (no external JS/CSS dependencies; all fonts and images are base64-inlined)
- GitHub Pages — the repo root is served as the site
- Custom domain: `preview.midasdecks.com` (set via `CNAME` file)
- Branch: `master`
- Remote: `git@github.com:D-RAJ65/midas-previews.git`

## File Structure

```
CNAME               # Custom domain: preview.midasdecks.com
index.html          # Redirect to https://midasdecks.com (not a real landing page)
<8char-hex>.html    # One file per published deck preview (e.g. c97404fa.html)
```

Each `<8char-hex>.html` file is a standalone slide viewer at 960×540px (16:9) with:
- Keyboard navigation (arrow keys, space)
- Prev/Next HUD buttons
- Slide counter
- Animated preloader
- All fonts and images inlined as base64

## How Files Get Here

Files are never created manually. They are published via the `--publish` flag in `deckify.py`:

```bash
# From /home/dmahtani/Coding/deckify/
python deckify.py input/<project>/ --publish
```

That command:
1. Generates a fully self-contained HTML file via `deckify.py`'s `share()` method
2. Copies it to this repo with a random 8-character hex ID as the filename
3. Runs `git add`, `git commit`, and `git push` automatically
4. Prints the live URL: `https://preview.midasdecks.com/<hex-id>.html`

## Manual Deployment (if needed)

If a file was added but not pushed, or you need to re-push:

```bash
cd /home/dmahtani/Coding/midas-previews
git add <filename>.html
git commit -m "publish: <deck name>"
git push
```

GitHub Pages deploys automatically on push to `master`. No build action required.

## Removing a Preview

To revoke a shared link:

```bash
cd /home/dmahtani/Coding/midas-previews
git rm <hex-id>.html
git commit -m "remove: <deck name>"
git push
```

The URL will return a 404 once GitHub Pages rebuilds (usually within seconds).

## What NOT to Do

- Do not edit the HTML files directly — they are generated output, not source files
- Do not modify `index.html` to be anything other than a redirect — it only exists to avoid a bare 404 on the root domain
- Do not change the `CNAME` file — it controls the custom domain binding
- Do not add a Jekyll config, package.json, or any build tooling — this repo must stay a flat static site
- Do not commit large binary files — all assets must be inlined in the HTML by `deckify.py` before publishing

## Related Projects

| Repo | Path | Role |
|---|---|---|
| deckify | `/home/dmahtani/Coding/deckify/` | Source of all HTML previews; contains `deckify.py` and `CLAUDE.md` with full build instructions |
| midasdecks.com | `/home/dmahtani/Coding/midasdecks.com/` | Main marketing site that `index.html` redirects to |
