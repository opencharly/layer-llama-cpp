# AGENTS.md — layer-llama-cpp

Standalone candy repo for the `llama-cpp` layer — prebuilt llama.cpp binaries and
the GGUF conversion tools. The candy lives in `charly.yml` at the repo root: the
`LLAMA_CPP_PATH` env / `path_append`, the download plan step, the `check:`
assertions, and the embedded `skill:` entity projected into the marketplace
corpus as `/charly-jupyter:llama-cpp`.

Canonical files:

- `charly.yml` — the `llama-cpp:` candy entity and the `llama-cpp-skill:` skill
  entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-jupyter:llama-cpp` — the owning skill. The binaries, the GGUF
  conversion tools, the `LLAMA_CPP_PATH` env var, and the Tier 1 post-install
  pattern. Load before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `command:`/`check:`, per-distro `distro:` arms,
  package/repo sections, and service declarations). Load before editing any
  entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; they must stay
  valid on every distro arm they run on. Scope a distro-specific check in the
  command itself — the check runner does not honour runner-level
  `exclude-distro` fields.
- This candy has no `pixi.toml` and no `require:` — it is deliberately Tier 1
  and composed by an environment-owning candy.

## Modify this repo

- Edit the `llama-cpp:` candy entity AND the `llama-cpp-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a behaviour
  change not mirrored in the skill leaves the corpus stale.
- The release-selection loop picks the newest release that ships an `ubuntu-x64`
  binary and reuses that tag for the source tree — keep the single selection so
  binaries and GGUF tools cannot drift.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
