# Revert before final PR

## CODEOWNERS vs write/admin (reference)
- **write/admin:** Repo permission. User has push/collab access. Checked via `repos.getCollaboratorPermissionLevel` → `admin` or `write`.
- **CODEOWNERS:** Names in `CODEOWNERS` file. Per-path ownership; we use “is user in any CODEOWNERS line?” for triage. No API for “is CODEOWNER?” — we read the file and parse `@username`.
- **Why switch:** Maintainer asked to use CODEOWNERS. It can differ from write/admin (e.g. docs CODEOWNERS vs org admins). Test CODEOWNERS-based flows last.

---

## 1. Fork / token (all 8 triage workflows)
- Remove `|| github.repository == 'Manancode/.github'` from `if` conditions.
- Optionally change `secrets.GH_TOKEN || secrets.GITHUB_TOKEN` back to `secrets.GH_TOKEN` (or keep fallback if maintainers prefer).

## 2. Issue Triage Reminders – timing
- **File:** `.github/workflows/issue-triage-reminders.yml`
- **Revert:** Replace TEST MODE timing (minutes) with production timing (days).
- Production values: `twoWeeksAgo` 14d, `threeWeeksAgo` 21d, `fiveWeeksAgo` 35d, `sixWeeksAgo` 42d, `sixtyDaysAgo` 60d.
- Restore reminder messages to use "2-3 weeks", "5-6 weeks", "60 days".

## 3. PR Reminders – timing
- **File:** `.github/workflows/pr-reminders.yml`
- **Revert:** Replace TEST MODE timing (minutes) with production timing (days).
- Production: `oneWeekAgo` 7d, `twoWeeksAgo` 14d, `threeWeeksAgo` 21d, `fourWeeksAgo` 28d.
- Restore reminder messages to use "1-2 weeks", "2-3 weeks", "3-4 weeks".

## 4. CODEOWNERS testing
- Test auto-triage on CODEOWNER comment **last** (add self to CODEOWNERS temporarily if needed).
- Remove self from CODEOWNERS after testing.

## 5. Test branch
- After PR testing, delete branch `test-pr-triage`.
