# Copilot instructions

## Big picture
- This repository is a PreTeXt book project. Build targets are declared in `project.ptx` (`web` HTML, `print` PDF).
- Document composition is include-driven: `source/main.ptx` includes `docinfo`, frontmatter, chapter files (`01-vs.ptx`, `02-iso.ptx`, `99-appendices.ptx`), then `backmatter`.
- Chapter files include section files with `xi:include` (example: `source/02-iso.ptx` includes `02-01.ptx`, `02-02.ptx`, `02-03.ptx`).

## Authoring flow and boundaries
- Edit only `source/*.ptx` for content changes.
- Keep `xml:id` stable; HTML output filenames and cross-references are derived from these IDs.
- Do not edit generated output in `output/` (`output/web`, `output/stage`) by hand.
- Asset ownership is split by config in `publication/publication.ptx`:
  - external assets: `assets/`
  - generated assets: `generated-assets/`

## Build, preview, and validation
- Python dependency is pinned in `requirements.txt`: `pretext == 2.11.3`.
- Typical local setup:
  - `pip install -r requirements.txt`
  - `pretext --version`
- Full HTML build: `pretext build web`
- Focused preview build (used by CodeChat):
  - `python -m pretext build web --xmlid <xml-id> --no-knowls`
  - configured in `codechat_config.yaml` as `--xmlid {xml_id} --no-knowls`
- PDF build target exists via manifest: `pretext build print`

## CI and deployment integration
- CI workflow is in `.github/workflows/pretext-cli.yml` and runs inside `oscarlevin/pretext:full`.
- Build logic is version-gated:
  - PreTeXt >= 2.5: `pretext build --deploys`
  - older versions: `pretext build`
- Deployment staging is always generated with `pretext deploy --stage-only` to `output/stage`.
- `output/stage` is uploaded as artifact `deploy`, then optionally published:
  - Cloudflare Pages when `CLOUDFLARE_PROJECT_NAME` is set
  - GitHub Pages when `PTX_ENABLE_DEPLOY_GHPAGES == 'yes'` on default branch

## Project-specific editing conventions
- Prefer adding new section/chapter content as new `source/*.ptx` files, then wire them via `xi:include` in the parent file.
- Keep presentation/system behavior changes in `publication/publication.ptx` (chunking, TOC, knowls, HTML style), not in chapter text files.
- Existing prose is Korean with PreTeXt math/structure tags (`<m>`, `<theorem>`, `<proof>`, `<example>`, `<question>`); preserve this authoring style when extending content.
