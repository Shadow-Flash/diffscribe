# Diffscribe

> **Turns your git diff into a complete, human-quality PR description in seconds.**

Diffscribe is a Claude skill that reads your current branch's actual work git diff, commit messages, changed files, and branch name and intelligently fills out your Pull Request or Merge Request template. No more writing PR descriptions from scratch.

![GitHub](https://img.shields.io/badge/GitHub-✓-black?logo=github)
![GitLab](https://img.shields.io/badge/GitLab-✓-orange?logo=gitlab)
![Bitbucket](https://img.shields.io/badge/Bitbucket-✓-blue?logo=bitbucket)
![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-✓-0078D4?logo=azure-devops)
![License: MIT](https://img.shields.io/badge/License-MIT-green)

---

## ✨ What It Does

| Input                    | Output                                   |
| ------------------------ | ---------------------------------------- |
| Your PR template path    | Every section filled intelligently       |
| Your current git branch  | Summary, motivation, what changed        |
| Commit messages          | Testing notes, checklist auto-checked    |
| Git diff + changed files | Breaking change detection, issue linking |

---

## 🚀 Quick Start

### 1. Install the Skill

```
npx skills add Shadow-Flash/diffscribe
```
**OR**

Download [`diffscribe.skill`](./diffscribe.skill) and install it into Claude:

- In **Claude Code**: Drop the `.skill` file into your project's `.claude/skills/` directory
- In **Claude.ai**: Upload via the Skills settings panel

### 2. Invoke Diffscribe

Just talk to Claude naturally:

```
Fill my PR template at .github/pull_request_template.md using my current branch vs develop
```

```
Use Diffscribe, my template is at docs/mr_template.md
```

```
Auto-generate my pull request description from what I've built on this branch
```

### 3. Copy & Paste

Diffscribe outputs a ready-to-paste PR description. Optionally, it can raise the PR directly via CLI.

---

## 📋 Example

**Branch:** `feat/123-add-oauth-google-login`
**Template:** `.github/pull_request_template.md`

<details>
<summary>View sample output →</summary>

See [`sample_output.md`](./sample_output.md) for a full example of a Diffscribe-generated PR description.

</details>

---

## ⚙️ Parameters

| Parameter        | Required | Default           | Description                      |
| ---------------- | -------- | ----------------- | -------------------------------- |
| `template_path`  | ✅ Yes    | —                 | Path to your PR/MR template file |
| `base_branch`    | Optional | `main` → `master` | Branch to diff against           |
| `max_diff_lines` | Optional | `300`             | Max lines of diff to analyze     |

---

## 🌍 Platform Support

Diffscribe works with **any git platform** — it reads your local git state, so it's platform-agnostic by design.

| Platform         | Template Location                            | CLI Command            |
| ---------------- | -------------------------------------------- | ---------------------- |
| **GitHub**       | `.github/pull_request_template.md`           | `gh pr create`         |
| **GitLab**       | `.gitlab/merge_request_templates/Default.md` | `glab mr create`       |
| **Bitbucket**    | `docs/pull_request_template.md`              | Bitbucket API / web UI |
| **Azure DevOps** | `.azuredevops/pull_request_template.md`      | `az repos pr create`   |

See [`diffscribe/references/platform-notes.md`](./diffscribe/references/platform-notes.md) for full CLI examples.

---

## 📁 Repository Structure

```
diffscribe/
├── README.md                          ← You are here
├── LICENSE                            ← MIT
├── diffscribe.skill                   ← Packaged skill (install this)
├── diffscribe/
│   ├── SKILL.md                       ← Skill source & instructions
│   └── references/
│       └── platform-notes.md          ← Platform-specific CLI tips
├── sample_pr_template.md          ← Example PR template
└── sample_output.md               ← Example Diffscribe output
```

---

## 💡 Tips for Best Results

- **Descriptive branch names** give Diffscribe better context:
  `feat/123-oauth-google-login` produces a far richer description than `dev-branch-2`
- **Atomic commits** with clear messages = better summaries
- Lock files are automatically excluded from diff analysis
- For very large PRs (1000+ changed lines), set `max_diff_lines=500`

---

## 🤝 Contributing

Contributions welcome! To improve Diffscribe:

1. Fork this repo
2. Edit `diffscribe/SKILL.md` with your improvements
3. Test against a few real branches
4. Open a PR with before/after examples

---

## 📄 License

MIT — see [LICENSE](./LICENSE)

---

## 🙏 Acknowledgements

Built as a Claude skill using [Anthropic's Claude](https://claude.ai). Diffscribe is community-maintained and not officially affiliated with Anthropic.

---

**If Diffscribe saves you time, give it a ⭐, it helps others discover it!**
