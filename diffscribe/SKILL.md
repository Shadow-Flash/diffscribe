---
name: diffscribe
description: >
  Diffscribe reads your git branch — diff, commit messages, changed files, and branch name —
  and automatically writes a fully filled Pull Request or Merge Request description using your
  existing PR template. Use this skill whenever a user mentions: "fill my PR template",
  "write my pull request description", "fill out my MR", "generate PR description from branch",
  "auto-fill PR", "use Diffscribe", "create PR from branch work", or any request to populate
  a PR/MR template using code changes. Works with GitHub, GitLab, Bitbucket, Azure DevOps,
  or any git platform. The user provides the path to their PR template and optionally the base
  branch to diff against. Always trigger Diffscribe for PR/MR description generation tasks —
  even if the user doesn't say "skill" explicitly.
---

# Diffscribe

> **Turns your git diff into a complete, human-quality PR description.**

Diffscribe reads your branch's actual work — commit messages, code diff, changed files, branch name — and intelligently fills every section of your PR/MR template. Works on any git platform, any template format.

---

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `template_path` | ✅ Yes | — | Path to your PR/MR template (e.g. `.github/pull_request_template.md`) |
| `base_branch` | Optional | `main` → `master` | Branch to diff against |
| `max_diff_lines` | Optional | `300` | Truncate large diffs to keep context clean |

---

## Execution Steps

### Step 1 — Gather Inputs

If not already provided by the user, ask:
- What is the path to your PR template?
- What base branch to compare against? (default: `main`)

### Step 2 — Read the PR Template

```bash
cat <template_path>
```

Parse all sections. Sections are delimited by:
- Markdown headers (`## Section`, `### Section`)
- HTML comment prompts (`<!-- Describe your changes -->`)
- Checkbox items (`- [ ] item`)
- Freeform placeholder text

### Step 3 — Collect Branch Context

Run all four commands:

```bash
# 1. Branch name
git rev-parse --abbrev-ref HEAD

# 2. Commit messages (since base branch, no merge commits)
git log <base_branch>..HEAD --oneline --no-merges

# 3. Changed files with status
git diff <base_branch>..HEAD --name-status

# 4. Code diff — lock files excluded, truncated
git diff <base_branch>..HEAD -- . ':(exclude)*.lock' ':(exclude)package-lock.json' | head -n <max_diff_lines>
```

**Base branch fallback:**
1. Try `main`
2. If it fails, try `master`
3. If both fail, use `git log --oneline -20` and note the fallback in output

### Step 4 — Fill The Template

Map branch context to every section intelligently:

| Section Type | Filling Strategy |
|---|---|
| **Summary / Description** | 2–4 sentences from commit messages + diff intent |
| **What changed** | Bullet list from `--name-status`, grouped by Added / Modified / Deleted |
| **Why / Motivation** | Infer from branch name + commit messages (e.g. `fix/401-token-expiry` → fixing token expiry bug) |
| **How it works** | Brief technical explanation derived from the diff |
| **Testing** | List test files changed; if none, note "manual testing recommended" |
| **Checklist** (`- [ ]`) | Auto-check items that clearly apply; leave uncertain ones unchecked |
| **Screenshots** | Leave as `<!-- Add screenshots if applicable -->` |
| **Breaking changes** | Flag if public APIs, exports, or interfaces were modified |
| **Related issues** | Extract numbers from branch name or commits (e.g. `feat/123-login` → `Closes #123`) |
| **Reviewer notes** | Add "Key files to review" based on most impactful changes |

**Tone:** Professional, concise, developer-friendly. First person voice ("This PR adds…", "This fixes…").

### Step 5 — Output

Present the filled description in a clean block:

```
## ✅ Diffscribe Output — Ready to Paste

---
[filled PR description]
---
```

Then offer:
> "Want me to raise this as a PR via CLI (`gh pr create` / `glab mr create` / `az repos pr create`)?"

---

## Error Handling

| Situation | Response |
|---|---|
| Template not found | "Could not find template at `<path>`. Please confirm the path." |
| No commits ahead of base | "No commits found ahead of `<base_branch>`. Are you on the right branch?" |
| Empty diff | Fill from branch name + commits only; note the limitation |
| No clear template sections | Treat entire file as one Description block |
| Very large diff | Truncate at `max_diff_lines`; note in output |

---

## Example Phrases That Trigger Diffscribe

- *"Fill my PR template at `.github/pull_request_template.md` vs `develop`"*
- *"Use Diffscribe on my current branch"*
- *"Auto-generate my pull request description"*
- *"My MR template is at `docs/mr_template.md`, base is `staging`"*
- *"Write my PR description from what I've built"*

---

## Reference

See `references/platform-notes.md` for platform-specific CLI commands (GitHub, GitLab, Bitbucket, Azure DevOps).
