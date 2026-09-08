# Git Commit Auto

GitHub Actions workflow that creates automated activity commits on a schedule.
It can also be started manually from the repository's **Actions** tab.

## How It Works

The workflow in `.github/workflows/daily-commit.yml`:

1. Runs every hour at minute 0 in UTC.
2. Checks out the repository with full Git history.
3. Writes a UTC timestamp to `activity.log`.
4. Creates 7 commits for that run.
5. Pushes the commits back to the repository's `main` branch.

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
    - cron: "0 * * * *"
```

Schedules use UTC. The default expression runs once every hour at minute 0.

## Customize the Commit Count

Change the loop range to create a different number of commits:

```bash
for i in {1..7}; do
```

For example, `{1..5}` creates five commits per run, while `{1..7}` creates seven per run.

## Notes

- Each run appends to `activity.log` and commits the file.
- Scheduled workflows may be delayed during periods of high GitHub Actions load.
- Frequent automated commits can make repository history noisy. Use this workflow only where that history is intentional.
- Scheduled workflows are automatically disabled by GitHub after 60 days of repository inactivity. Push a manual commit occasionally, or re-enable the workflow from the Actions tab if this happens.
