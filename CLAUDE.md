# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

User documentation for the `kinematic-tracker` Python package (multiple-object tracking; main class `NdKkfTracker`, published on PyPI). The tracker's source code lives in a separate repository — this repo only holds the docs, published at https://kovalp.github.io/kinematic-tracker-docs/.

It is two things side by side:

1. **A Jekyll site for GitHub Pages** (the actual docs): Markdown pages at the root and in `association/`, rendered with the remote `just-the-docs` theme (`_config.yml`). GitHub Pages builds it; there is no CI workflow in the repo.
2. **A small `uv` Python project** (`pyproject.toml`, `uv.lock`) used to run and test the example notebooks in `examples/` against the published `kinematic-tracker` package. `src/kinematic_tracker_docs/` is only a placeholder stub.

## Commands

Jekyll site (needs Ruby; see `dev/README.md`):

```shell
bundle install
bundle exec jekyll serve   # http://127.0.0.1:4000
```

Notebook tests (Python >= 3.14.2, via `uv`):

```shell
uv run pytest --nbmake examples/                            # all notebooks
uv run pytest examples/ex001_start.ipynb --nb-test-files    # single notebook (pytest-notebook)
```

## Writing docs pages

- Math is rendered by MathJax, configured in `_includes/head_custom.html` (also custom code-block colors). Use `$...$` inline and `$$...$$` display math; kramdown is set to GFM input with `math_engine: mathjax`.
- `index.md` holds the table of contents and the "latest version of `kinematic-tracker`" line. Link pages without the `.md` extension (e.g. `[Settings](settings)`; from `association/` use `../track-nu-scenes`).
- `settings.md` is the hub for tracker setters; the pages under `association/` (association metrics such as Mahalanobis / GIoU variants, and the matching algorithm) are linked from `settings.md` and `association/association-metric.md`. New metric pages should be added to those lists.
- Pages that should not appear in navigation/search use front matter with `nav_exclude: true` and `search_exclude: true` (see `dev/README.md`).

## Release bookkeeping

When a new `kinematic-tracker` version is released, the docs are updated by adding an entry at the top of `CHANGELOG.md` (`# <version>` followed by a bullet) and bumping the version string in `index.md`. These two files change together.
