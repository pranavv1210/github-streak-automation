# GitHub Streak Maintainer 🔥

Automatically maintain your GitHub contribution streak using GitHub Actions. Never miss a day again!

## Overview

This repository uses GitHub Actions to automatically commit changes every day at **12:05 AM IST (6:35 PM UTC)**, ensuring you maintain a continuous contribution streak without any manual effort.

The workflow runs on GitHub's servers, so your machine doesn't need to be on. Perfect for keeping your GitHub profile active 24/7!

## Features

✨ **Fully Automated**
- Runs automatically every day at a scheduled time
- No manual intervention required

🤖 **Smart Commits**
- Random commit messages for variety
- Automatic timestamp logging
- Handles edge cases gracefully

🛡️ **Production Ready**
- Works 100% on GitHub servers
- Includes error handling
- Never fails even if there are no changes

⚙️ **Easy Customization**
- Simple cron schedule configuration
- Easily adjust timing to your preference

## Setup Instructions

### 1. Repository Setup

This repository is already configured as `github-streak-automation` in your GitHub account.

### 2. Verify Repository Settings

Ensure the following is configured in your repository:

**Settings → Actions → General**
- ✅ "Allow all actions and reusable workflows" (or configure as needed)
- ✅ "Read and write permissions" is enabled for workflows

**Settings → Actions → General → Workflow Permissions**
- ✅ "Read and write permissions"
- ✅ "Allow GitHub Actions to create and approve pull requests" (optional)

### 3. Understanding the Workflow

The workflow is located in `.github/workflows/streak.yml` and does the following:

1. **Checkout**: Pulls the latest repository code
2. **Configure Git**: Sets up Git credentials for commits
3. **Update File**: Appends current timestamp to `streak.txt`
4. **Random Message**: Selects a random motivational commit message
5. **Random Delay**: Adds 0-30 second delay before commit
6. **Check Changes**: Verifies if there are changes to commit
7. **Commit**: Stages and commits the changes
8. **Push**: Pushes changes back to GitHub
9. **Complete**: Workflow finishes (never fails)

### 4. Manual Trigger

You can also manually trigger the workflow:

1. Go to **Actions** tab
2. Select **GitHub Streak Automation** workflow
3. Click **Run workflow** → **Run workflow**

## Workflow Permissions

The workflow uses the following permission:

```yaml
permissions:
  contents: write
```

This allows the workflow to:
- ✅ Read repository contents
- ✅ Create commits
- ✅ Push changes to main branch

GitHub automatically provides the necessary authentication token (`GITHUB_TOKEN`) for these operations.

## How the Automation Works

### Daily Schedule

The workflow runs on a **cron schedule**:

```yaml
- cron: '35 18 * * *'
```

**Cron Format**: `minute hour day month day-of-week`

- `35` = minute (35)
- `18` = hour in UTC (18:00 UTC = 6:35 PM UTC)
- `*` = any day of month
- `*` = any month
- `*` = any day of week

**Time Conversion**:
- UTC: 6:35 PM (18:35)
- IST (UTC+5:30): 12:05 AM (next day)

### Git Configuration

The workflow automatically configures Git:

```bash
git config --global user.name "github-actions[bot]"
git config --global user.email "41898282+github-actions[bot]@users.noreply.github.com"
```

This uses GitHub's official bot account for commits.

### Commit Messages

The workflow randomly selects from this list of messages:

- 🔥 Maintaining the streak!
- ⚡ Keep the momentum going
- 🎯 Daily commitment to code
- ✨ Another day, another commit
- 💪 Streak strong and growing
- 🚀 Consistency is key
- 📈 Building great habits
- 🎊 Keeping contributions flowing
- ⭐ One more day of progress
- 🌟 Never miss a day

Add your own messages by editing the `MESSAGES` array in the workflow!

### Smart Error Handling

The workflow includes error handling to ensure it never fails:

- **No Changes Check**: If no changes exist, the workflow completes successfully
- **Continue on Error**: Push operation uses `continue-on-error: true`
- **Graceful Completion**: Final step always succeeds

## Customization

### Change the Schedule Time

Edit `.github/workflows/streak.yml` and modify the cron expression:

```yaml
on:
  schedule:
    - cron: '35 18 * * *'  # Change this
```

**Common Schedule Examples**:

| Time | Cron Expression | Notes |
|------|-----------------|-------|
| 12:05 AM IST | `35 18 * * *` | 6:35 PM UTC previous day |
| 1:00 AM IST | `30 19 * * *` | 7:30 PM UTC previous day |
| 9:00 AM IST | `30 03 * * *` | 3:30 AM UTC same day |
| Every 6 hours | `0 */6 * * *` | Runs at 0, 6, 12, 18 UTC |
| Every hour | `0 * * * *` | Runs hourly |

💡 **Tip**: Use [crontab.guru](https://crontab.guru) for easy cron scheduling!

### Customize Commit Messages

Edit `.github/workflows/streak.yml` and modify the `MESSAGES` array:

```bash
MESSAGES=(
  "🔥 Maintaining the streak!"
  "Your custom message here"
  "Another message"
)
```

### Change the Tracked File

Edit `.github/workflows/streak.yml` - change `streak.txt` to any filename you want:

```yaml
- name: Update streak file
  run: |
    echo "$(date -u +'%Y-%m-%d %H:%M:%S UTC')" >> your-file-name.txt
```

### Adjust Random Delay

The workflow adds a random 0-30 second delay. Modify in `.github/workflows/streak.yml`:

```bash
sleep $((RANDOM % 30))  # Change 30 to your preferred max seconds
```

### Change Default Branch

If your default branch isn't `main`, edit the workflow to use your branch name:

```yaml
git push origin your-branch-name
```

## File Structure

```
github-streak-automation/
├── .github/
│   └── workflows/
│       └── streak.yml          # Main workflow file
├── streak.txt                  # Streak tracking log
├── .gitignore                  # Git ignore rules
└── README.md                   # This file
```

## Monitoring the Workflow

### View Workflow Runs

1. Go to your repository
2. Click **Actions** tab
3. Select **GitHub Streak Automation**
4. View all runs with timestamps and status

### Check Logs

1. Click on a specific run
2. Expand the `maintain-streak` job
3. View detailed step-by-step logs

### Verify Commits

Check your repository's commit history to confirm daily commits are being made automatically.

## Troubleshooting

### Workflow Not Running

1. **Check if scheduled workflows are enabled**:
   - Settings → Actions → General
   - Ensure "Allow all actions and reusable workflows" is selected

2. **Verify permissions**:
   - Settings → Actions → General
   - Ensure "Read and write permissions" is enabled

3. **Check branch name**:
   - Ensure the workflow references the correct branch name (default: `main`)

### Commits Not Appearing

1. **Check permissions**: Ensure the workflow has write access to contents
2. **Verify default branch**: Confirm the branch name in the workflow matches your repository's default branch
3. **Check git configuration**: View workflow logs for any errors during git setup

### Random Delay Taking Too Long

The delay is 0-30 seconds by default. Reduce it if needed:

```bash
sleep $((RANDOM % 10))  # 0-10 seconds instead
```

## Security Considerations

✅ **Safe and Secure**:
- Uses GitHub's official bot account
- Only writes to your own repository
- No external APIs or third-party services
- Permissions are minimal and well-scoped

⚠️ **Best Practices**:
- Don't commit sensitive information to this repository
- This file is public, so anyone can see your commit history
- Use branch protection rules if needed

## FAQ

**Q: Will this affect my real contributions?**
A: No! The commits are separate from your actual coding work. Use this for the streak automation only.

**Q: Can I adjust the time?**
A: Yes! Edit the cron expression in `.github/workflows/streak.yml`. Use [crontab.guru](https://crontab.guru) for help.

**Q: What if GitHub is down?**
A: The workflow will be retried automatically. Scheduled workflows have built-in retry mechanisms.

**Q: Can I run it manually?**
A: Yes! Click **Actions** → **GitHub Streak Automation** → **Run workflow** to trigger manually.

**Q: Does my laptop need to be on?**
A: No! The workflow runs on GitHub's servers, so it works 24/7 regardless of your laptop's status.

**Q: Can I change the file that gets updated?**
A: Yes! Edit the workflow to append to any file you want. Just make sure it exists in the repository.

## License

This repository is provided as-is for personal use.

## Support

For issues or questions:
1. Check the troubleshooting section above
2. Review workflow logs in the Actions tab
3. Verify all setup steps are completed

---

**Happy streaking! 🔥** Keep your GitHub contributions growing every single day!
