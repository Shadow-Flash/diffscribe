# Platform-Specific Notes

## GitHub
- Default template: `.github/pull_request_template.md`
- Multiple templates: `.github/PULL_REQUEST_TEMPLATE/<name>.md`
- Raise PR via CLI:
  ```bash
  gh pr create --title "Your Title" --body "$(cat filled_pr.md)"
  ```

## GitLab
- Default template: `.gitlab/merge_request_templates/Default.md`
- Multiple templates: `.gitlab/merge_request_templates/<name>.md`
- Raise MR via CLI:
  ```bash
  glab mr create --title "Your Title" --description "$(cat filled_pr.md)"
  ```

## Bitbucket
- Template location: `docs/pull_request_template.md` (custom per repo)
- Use Bitbucket API or web UI to raise PRs with the filled description

## Azure DevOps
- Default template: `.azuredevops/pull_request_template.md`
- Multiple templates: `.azuredevops/pull_request_template/<name>.md`
- Raise PR via CLI:
  ```bash
  az repos pr create --title "Your Title" --description "$(cat filled_pr.md)"
  ```

## Tips for Best Results

- **Descriptive branch names** produce better descriptions: `feat/123-oauth-login` > `dev-branch-2`
- **Atomic commits** with clear messages = better summaries
- Lock files are excluded from diffs by default — keeps output clean
- For massive diffs (1000+ lines), set `max_diff_lines=500`
