# github-streak-maintainer

`github-streak-maintainer` is a small GitHub Actions repository that keeps a contribution streak active by making a harmless daily update to `streak.txt`, committing it, and pushing it back to the repository on GitHub's servers.

## What it does

- Runs automatically every day at 12:05 AM IST using the cron schedule `35 18 * * *`.
- Supports manual runs from the GitHub Actions tab.
- Appends a UTC timestamp to `streak.txt`.
- Creates a commit only when the file changed.
- Pushes the commit back to the default branch with safe, authenticated GitHub Actions permissions.
- Includes a small random delay and a short list of commit messages to make the automation less repetitive.

## How the automation works

The workflow in [.github/workflows/streak.yml](.github/workflows/streak.yml) runs on GitHub-hosted Ubuntu runners. It checks out the repository, configures Git identity, appends the current timestamp to `streak.txt`, waits a random few minutes, selects a commit message, and then commits and pushes only when there is something to commit.

Because the entire workflow runs on GitHub infrastructure, your laptop can be turned off and the schedule will still run.

## Repository structure

```text
.github/workflows/streak.yml
.gitignore
README.md
streak.txt
```

## Workflow permissions

This repository uses the minimum permission needed for automatic commits:

```yaml
permissions:
  contents: write
```

To make this work in your repository, go to:

1. `Settings`
2. `Actions`
3. `General`
4. `Workflow permissions`
5. Select `Read and write permissions`

If the repository is newly created or forked, also make sure GitHub Actions are enabled for the repository.

## How to fork and use it

1. Fork this repository into your GitHub account.
2. Open the forked repository on GitHub.
3. Go to `Settings > Actions > General` and enable `Read and write permissions`.
4. Confirm that the workflow file exists at [.github/workflows/streak.yml](.github/workflows/streak.yml).
5. Push or edit the repository once if GitHub asks you to enable Actions for the fork.
6. Open the `Actions` tab and run the workflow manually once if you want to confirm the setup immediately.

If you keep the default branch name as `main`, the workflow will push back to that branch automatically.

## GitHub Actions setup instructions

1. Make sure the repository has Actions enabled.
2. Set workflow permissions to `Read and write permissions`.
3. Save the settings.
4. Visit the `Actions` tab.
5. Run the workflow manually the first time to confirm that commits and pushes work.

## Customizing the commit timing

The schedule uses UTC cron syntax because GitHub Actions schedules are evaluated in UTC.

Current schedule:

```yaml
- cron: '35 18 * * *'
```

That corresponds to 12:05 AM IST.

To change the time, update the cron expression in the workflow file. A few examples:

- `35 18 * * *` for 12:05 AM IST
- `30 19 * * *` for 1:00 AM IST
- `0 0 * * *` for 5:30 AM IST

The workflow also includes a random delay before committing. If you want to make it shorter or longer, edit the delay step in [.github/workflows/streak.yml](.github/workflows/streak.yml).

## Customizing commit messages

The workflow uses a small list of commit messages, including the required base message `Daily streak update`. You can edit the message array in [.github/workflows/streak.yml](.github/workflows/streak.yml) to add, remove, or simplify the options.

## Notes

- The tracked file is `streak.txt`.
- The workflow avoids failure when there are no changes to commit.
- This repository is intended as a beginner-friendly template and can be forked as-is.