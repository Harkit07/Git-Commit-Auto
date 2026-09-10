# Git Commit Auto

GitHub Actions workflow that creates automated activity commits on a schedule.
It can also be started manually from the repository's **Actions** tab.

## Folder Structure

```text
.
├── .github/
│   └── workflows/
│       └── daily-commit.yml  # Scheduled GitHub Actions workflow
├── activity.log              # Timestamp updated by the workflow
└── Readme.md                 # Project documentation
```

## How It Works

The workflow in `.github/workflows/daily-commit.yml`:

1. Runs daily at 3:30 UTC (9:00 AM IST) and 11:30 UTC (5:00 PM IST).
2. Checks out the repository with full Git history.
3. Configures the Git identity using optional repository secrets.
4. Appends a UTC timestamp and commit number to `activity.log`.
5. Creates 150 commits for that run.
6. Pushes the commits back to the repository's `main` branch.

## Setup

1. Push this repository to GitHub.
2. Optionally go to **Settings → Secrets and variables → Actions** and add:
   - `GIT_NAME` — the name to attribute commits to
   - `GIT_EMAIL` — a verified email on your GitHub account (required for commits to count toward your contribution graph). Verify it at `github.com/settings/emails` if it isn't already.
   
   If these secrets are not set, the workflow falls back to the GitHub actor name and a noreply email automatically.
3. Go to **Settings → Actions → General → Workflow permissions** and select **Read and write permissions**. This is required for the workflow to push commits back to the repository.
4. Ensure Actions are enabled for the repository.
5. Open **Actions**, choose the workflow from the list, and select **Run workflow** to test it manually.

The workflow uses the default `GITHUB_TOKEN` provided by GitHub Actions to push changes.

## Customize the Schedule

Update the `cron` expression in the workflow to change when the job runs:

```yaml
on:
  schedule:
    - cron: "30 3 * * *"
    - cron: "30 11 * * *"
```

Schedules use UTC. The default expressions run daily at `3:30 UTC` (`9:00 AM
IST`) and `11:30 UTC` (`5:00 PM IST`).

## Customize the Commit Count

Change the loop range to create a different number of commits:

```bash
for i in {1..150}; do
```

For example, `{1..5}` creates five commits per run, while `{1..150}` creates
150 per run.

## Notes

- Each run appends to `activity.log` and commits the file.
- Scheduled workflows may be delayed during periods of high GitHub Actions load.
- Frequent automated commits can make repository history noisy. Use this workflow only where that history is intentional.
- Scheduled workflows are automatically disabled by GitHub after 60 days of repository inactivity. Push a manual commit occasionally, or re-enable the workflow from the Actions tab if this happens.
