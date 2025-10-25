# Jellyfin Android TV ModernUI CI

This repository automatically monitors the [jellyfin-androidtv-modernui](https://github.com/akhilmulpurii/jellyfin-androidtv-modernui) repository for new commits and builds APK releases when changes are detected.

## How it works

### Daily Monitoring
- A scheduled GitHub Actions workflow runs every day at 2 AM UTC
- It checks the latest commit SHA from the target repository
- Compares it with the commit stored in `commit.txt`
- If they differ (or `commit.txt` doesn't exist), it triggers the build workflow

### Automatic Builds
- When a new commit is detected, the build workflow:
  - Clones the latest version of the target repository
  - Builds a debug APK using Gradle
  - Creates a new GitHub release with the APK attached
  - Updates `commit.txt` with the latest commit SHA

## Repository Structure

```
.github/workflows/
├── monitor-commits.yml    # Daily monitoring workflow
└── build-apk.yml         # APK build and release workflow

commit.txt               # Stores the last processed commit SHA
README.md               # This file
.gitignore             # Git ignore rules
```

## Setup Instructions

1. **Fork this repository** to your GitHub account

2. **Enable Actions** in your repository settings:
   - Go to Settings → Actions → General
   - Under "Actions permissions", select "Allow all actions and reusable workflows"

3. **Initial Setup**:
   - The first run will create `commit.txt` and build the initial APK
   - You can manually trigger the monitor workflow to start immediately

## Workflows

### Monitor Commits (`monitor-commits.yml`)
- **Trigger**: Daily at 2 AM UTC, or manual
- **Purpose**: Check for new commits in the target repository
- **Actions**:
  - Fetches latest commit SHA from `akhilmulpurii/jellyfin-androidtv-modernui`
  - Compares with stored commit in `commit.txt`
  - Triggers build workflow if changes detected
  - Updates `commit.txt` with new commit SHA

### Build APK (`build-apk.yml`)
- **Trigger**: Called by monitor workflow when commits change
- **Purpose**: Build APK and create release
- **Actions**:
  - Clones target repository at latest commit
  - Sets up Android build environment (JDK 17, Android SDK)
  - Builds debug APK using Gradle
  - Creates GitHub release with APK attachment
  - Includes commit information and installation notes

## Release Information

Each release includes:
- **APK file**: `jellyfin-androidtv-modernui-debug.apk`
- **Release notes**: Commit message, date, and installation instructions
- **Tag format**: `build-YYYY-MM-DD-xxxxxxxx` (date + short commit hash)

## Manual Triggers

You can manually trigger the monitoring workflow:
1. Go to Actions tab in your repository
2. Select "Monitor Jellyfin ModernUI Commits"
3. Click "Run workflow"

## Requirements

- GitHub repository with Actions enabled
- No additional setup required - everything runs automatically

## Target Repository

This CI monitors: **[akhilmulpurii/jellyfin-androidtv-modernui](https://github.com/akhilmulpurii/jellyfin-androidtv-modernui)**

A modern UI overhaul for the Jellyfin Android TV client featuring:
- Redesigned home interface with streaming service aesthetics
- Vertical content cards and interactive previews
- Enhanced content discovery
- Glassmorphic design elements

## Troubleshooting

### Build Failures
- Check the Actions tab for detailed logs
- Ensure the target repository's build process hasn't changed
- Verify Android SDK and build tools compatibility

### Permission Issues
- Make sure Actions are enabled in repository settings
- Check that `GITHUB_TOKEN` has appropriate permissions for releases

### Workflow Not Triggering
- Verify the schedule is correct (runs at 2 AM UTC)
- Check if there are any workflow file syntax errors
- Manually trigger the workflow to test

## Contributing

Feel free to:
- Report issues with the CI process
- Suggest improvements to the build process
- Modify the workflows for your needs

## License

This repository contains automation scripts. The built APKs are from the jellyfin-androidtv-modernui project, which is GPL-2.0 licensed.
