# Git Commit Auto

This repository contains a GitHub Actions workflow that automatically creates a large number of commit entries in `activity.log` on a schedule. It is designed for generating visible contribution activity in a GitHub repository without manually creating commits.

It can also be triggered manually from the repository's Actions tab.

## Project Structure

```text
.
├── .github/
│   └── workflows/
│       └── daily-commit.yml   # Daily auto-commit workflow
├── activity.log               # Log file updated by the workflow
├── Readme.md                  # Project documentation
└── .git/                      # Git metadata
```

## How the Workflow Works

The workflow in `.github/workflows/daily-commit.yml` does the following:

1. Runs on a daily cron schedule in UTC.
2. Checks out the repository with full history.
3. Configures git user details using optional repository secrets.
4. Appends a timestamp and commit counter to `activity.log`.
5. Creates 150 automatic commits for that run.
6. Pushes the generated commits to the repository's `main` branch.

## Schedule

The current workflow is configured to run at:

- `03:37 UTC` (approximately `09:07 AM IST`)
- `11:37 UTC` (approximately `05:07 PM IST`)

This is defined by the cron entries:

```yaml
on:
  schedule:
    - cron: "37 3 * * *"
    - cron: "37 11 * * *"
```

## Manual Trigger

You can also run the workflow manually from the GitHub UI:

1. Open the repository on GitHub.
2. Go to the **Actions** tab.
3. Select **Daily Auto Commit**.
4. Click **Run workflow**.

## Setup Instructions

1. Push this repository to GitHub.
2. Go to **Settings → Secrets and variables → Actions** and optionally add:
   - `GIT_NAME`: commit author name
   - `GIT_EMAIL`: a verified email address for your GitHub account
3. If the secrets are not set, the workflow falls back to:
   - `github.actor` as the name
   - a noreply GitHub email based on the actor ID
4. Ensure repository actions are enabled.
5. In **Settings → Actions → General**, set **Workflow permissions** to **Read and write permissions** so the workflow can push commits.

The workflow uses the default `GITHUB_TOKEN` from GitHub Actions to push updates.

## Commit Count

The workflow creates 150 commits per run with:

```bash
for i in {1..150}; do
```

To change this, update the loop range in the workflow file.

## Notes

- Each run appends to `activity.log` and commits the updated file.
- GitHub may delay scheduled jobs if there is high workflow load.
- Frequent automated commits can create a noisy repository history.
- If the repository is inactive for a long period, GitHub may disable scheduled workflows; a manual push or re-enabling the workflow can restore it.
