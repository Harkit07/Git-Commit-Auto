# Git Commit Auto

GitHub Actions workflow that creates automated activity commits on a schedule.
It can also be started manually from the repository's **Actions** tab.

## How It Works

The workflow in `.github/workflows/daily-commit.yml`:

1. Runs every day at 09:00 UTC.
2. Checks out the repository with full Git history.
3. Writes a UTC timestamp to `activity.log`.
4. Creates 10 commits for that day's run.
5. Pushes the commits back to the repository.

## Setup

1. Push this repository to GitHub.
2. Open `.github/workflows/daily-commit.yml`.
3. Replace the placeholder Git identity values:

	 ```yaml
	 git config user.name "Your Name"
	 git config user.email "YOUR_EMAIL@example.com"
	 ```

4. Ensure Actions are enabled for the repository.
5. Open **Actions**, select **Daily Auto Commit**, and choose **Run workflow** to test it manually.

The default `GITHUB_TOKEN` supplied by GitHub Actions is used to push changes. Repository settings may need to allow workflows to create and approve pull requests or make repository changes, depending on the repository's permissions policy.

## Customize the Schedule

Update the `cron` expression in the workflow to change when the job runs:

```yaml
on:
	schedule:
		- cron: "0 9 * * *"
```

Schedules use UTC. The default expression runs once per day at 09:00 UTC.

## Customize the Commit Count

Change the loop range to create a different number of commits:

```bash
for i in {1..10}; do
```

For example, `{1..5}` creates five commits per run.

## Notes

- Each run appends to `activity.log` and commits the file.
- Scheduled workflows may be delayed during periods of high GitHub Actions load.
- Frequent automated commits can make repository history noisy. Use this workflow only where that history is intentional.