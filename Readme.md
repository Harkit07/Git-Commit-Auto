# Git Commit Auto

GitHub Actions workflow that creates automated activity commits on a schedule.
It can also be started manually from the repository's **Actions** tab.

## How It Works

The workflow in `.github/workflows/daily-commit.yml`:

1. Runs every day at 04:29 UTC (09:59 IST).
2. Checks out the repository with full Git history.
3. Writes a UTC timestamp to `activity.log`.
4. Creates 60 commits for that day's run.
5. Pushes the commits back to the repository.

## Setup

1. Push this repository to GitHub.
2. Go to **Settings → Secrets and variables → Actions** and add two repository secrets:
   - `GIT_NAME` — the name to attribute commits to
   - `GIT_EMAIL` — a verified email on your GitHub account (required for commits to count toward your contribution graph). Verify it at `github.com/settings/emails` if it isn't already.
3. Go to **Settings → Actions → General → Workflow permissions** and select **Read and write permissions**. This is required for the workflow to push commits back to the repository.
4. Ensure Actions are enabled for the repository.
5. Open **Actions**, select **Daily Auto Commit**, and choose **Run workflow** to test it manually.

The default `GITHUB_TOKEN` supplied by GitHub Actions is used to push changes.

## Customize the Schedule

Update the `cron` expression in the workflow to change when the job runs:

```yaml
on:
  schedule:
    - cron: "29 4 * * *"
```

  Schedules use UTC. The default expression runs once per day at 04:29 UTC (09:59 IST).

## Customize the Commit Count

Change the loop range to create a different number of commits:

```bash
for i in {1..60}; do
```

For example, `{1..5}` creates five commits per run.

## Notes

- Each run appends to `activity.log` and commits the file.
- Scheduled workflows may be delayed during periods of high GitHub Actions load.
- Frequent automated commits can make repository history noisy. Use this workflow only where that history is intentional.
- Scheduled workflows are automatically disabled by GitHub after 60 days of repository inactivity. Push a manual commit occasionally, or re-enable the workflow from the Actions tab if this happens.
