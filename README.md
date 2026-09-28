# pytacheck-modules

The community store for [pytacheck](https://github.com/scienceverse/pytacheck):
**packs** of extra checks ("modules") and **presets** (which checks to run for a
field), contributed by researchers. pytacheck is the Python port of
[metacheck](https://github.com/scienceverse/metacheck), which checks research
papers for best practices.

This repository is a store: `index.json` lists every pack with its source commit,
a hash of its files, its modules, presets and review status. pytacheck reads it;
nothing here runs until you install a pack.

## Browse and install

```bash
pip install "pytacheck @ git+https://github.com/scienceverse/pytacheck@claude/elegant-fermat-eo7s89"   # not on PyPI yet
pytacheck pack search                       # everything in the store
pytacheck pack search trial --field medicine
pytacheck pack show clinical_trials         # details, without running any code
pytacheck pack install clinical_trials      # shows a consent card, then asks [y/N]
pytacheck init                              # or: pick presets for your field
```

Then run a pack's preset or module:

```bash
pytacheck run paper.json --preset fields::psychology
pytacheck run paper.json -m clinical_trials::trial_registration
```

| pack | kind | what it offers |
|---|---|---|
| [`fields`](packs/fields) | presets only | `fields::general`, `fields::psychology`, `fields::medicine`, `fields::open-science`: metacheck's checks chosen per field |
| [`clinical_trials`](packs/clinical_trials) | code | `trial_registration`: trial registry numbers (ClinicalTrials.gov, ISRCTN, EudraCT/CTIS, ANZCTR, ChiCTR, DRKS, CTRI, PACTR, UMIN/jRCT, IRCT, ReBEC, NTR) |

The full user guide is pytacheck's
[docs/MODULES.md](https://github.com/scienceverse/pytacheck/blob/claude/elegant-fermat-eo7s89/docs/MODULES.md).

> **While this store is private**, pytacheck needs read access to it: set
> `PYTACHECK_GITHUB_TOKEN` (or `GH_TOKEN` / `GITHUB_TOKEN`) to a GitHub token that
> can read this repository, for example
> `export PYTACHECK_GITHUB_TOKEN=$(gh auth token)`, or configure git credentials
> for github.com (for example `gh auth setup-git`). The token is sent only to
> GitHub and is never written to your config, install records or run records.

## What "reviewed" means

Maintainers read every pack before it is listed as reviewed, using the
[review checklist](REVIEW.md), and record the date in the index. A reviewed
pack is pinned to the exact commit and files that were reviewed: any change
clears the review date until it is reviewed again. Review is curation, not a
guarantee: packs run with your permissions, like any Python package. The
consent card pytacheck shows before installing lists every file and every
import the code makes.

## Contribute

Add a pack as a folder (`packs/<name>/`, no repository of your own needed) or
list your own repository (`packs/<name>.json`). See
[CONTRIBUTING.md](CONTRIBUTING.md).

## How this repository works

* `packs/<name>/` -- packs submitted as folders;
* `packs/<name>.json` -- packs in their authors' repositories (name, source,
  commit), plus maintainer-only `reviewed` / `reviewed_tree_sha256` / `yanked`
  fields;
* `store.json` -- the store's name and description;
* `index.json` -- **generated**: CI runs `pytacheck store build` on every push to
  `main` and commits the result. Do not edit it by hand.

Licence: each pack states its own licence in its `pack.json`. The repository's
own files (this README, the workflows) are CC0-1.0.
