# fleet — one Codespace for the whole fleet

Umbrella repo (owned by the personal account on purpose): the
developing-today fleet as git submodules, with a single devcontainer.

Why personal-account ownership matters: Codespaces **prebuilds** and the
repository Codespaces settings page are unavailable for org-owned repos
on the current organization plan (verified 2026-10-25 on
developing-today/code — Settings → Codespaces returns 404). A personal
repo under GitHub Pro gets prebuilds, so this umbrella is where the
fleet's codespaces live. The per-repo devcontainer in developing-today/
code (PR #855) still works for single-repo codespaces billed personally;
this repo is the prebuild play.

## Submodules

| Path | Repo |
|---|---|
| `code/` | developing-today/code |
| `opencode/` | developing-today/opencode |
| `openchamber/` | developing-today/openchamber |
| `nix/` | dezren39/nix |
| `zettel/` | developing-today/zettel |

All five are public, so clones inside a codespace need no extra auth.
The devcontainer requests write permission for the five repos at
codespace creation (approve the prompt) so pushes from inside work.

## Working in it

1. Create a codespace on this repo (2-core default; it's a big tree —
   the prebuild snapshot is what makes starts fast).
2. `cd <repo>` — submodules start **detached** at the recorded SHA.
   `git checkout main` (or your branch) before editing.
3. Commit and push **from inside the submodule**; that repo's own
   rules (worktrees, PRs) still apply.
4. Bumping the umbrella's recorded SHAs is a separate, optional commit
   here — do it when you want the fleet snapshot to move, e.g.
   `git submodule update --remote && git commit -am "sync fleet"`.

## Codespaces discipline (GitHub Pro: 180 core-hours + 20 GB storage/mo)

- Prebuilds: Settings → Codespaces → Prebuilds, trigger **on push**,
  branch `main`. They run on Actions minutes — free/unlimited here
  because this repo is public.
- **Stop, don't delete** between sessions; delete when a workstream
  ends. This codespace is large; watch the 20 GB storage meter.
- Secrets only via Codespaces secrets, never in the devcontainer config.
- Not for daemons (idle-stop) or GPU work (none; that's Modal's job).
