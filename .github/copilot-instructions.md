# Copilot instructions

## Project overview
- This is a PreTeXt book project. The root manifest is project.ptx, with targets for web (HTML) and print (PDF).
- The main source file is source/main.ptx, which assembles the book via xi:include of chapter/section files.
- Publication settings live in publication/publication.ptx (HTML/LaTeX options, asset directories).

## Key structure and data flow
- Edit content in source/*.ptx. source/main.ptx includes docinfo, frontmatter, chapters, and backmatter via xi:include.
- Assets live in assets/ and generated assets in generated-assets/; publication/publication.ptx maps these via <directories>.
- Build outputs are written to output/ (HTML at output/web, staging at output/stage). Do not hand-edit output/.

## Build and preview workflows
- Local CLI dependency is PreTeXt (requirements.txt pins pretext==2.11.3).
- The CodeChat previewer is configured in codechat_config.yaml and runs:
  pretext build web --xmlid {xml_id} --no-knowls
- CI ( .github/workflows/pretext-cli.yml ) installs deps, runs pretext build --deploys (or pretext build for older versions), then pretext deploy --stage-only to output/stage.

## Project-specific conventions
- Book structure is modular: add new content by creating a new source/*.ptx file and including it from source/main.ptx or a chapter file like source/01-vs.ptx.
- Publication options (chunking, TOC depth, knowl behavior, HTML theme) are adjusted only in publication/publication.ptx; avoid hard-coding presentation settings in content files.
- Keep generated assets and build artifacts out of edits; treat assets/ as author-managed and generated-assets/ as build-managed.

## Deployment targets
- CI uploads output/stage as an artifact; optional deployment to Cloudflare Pages or GitHub Pages is controlled by repo variables (see .github/workflows/pretext-cli.yml).
