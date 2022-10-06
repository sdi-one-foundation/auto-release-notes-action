# auto-release-notes-action
A composite Github Action for automatic release notes and notifications.

Example release.yaml that uses this action:

## Usage

1. Re-run all "etc" runs in the infrastructure repositories.  This will create the CHANGELOG.md file used for the release notes action.
2. Add the following "paths-ignore" section to all main-push.yaml files to prevent accidental staging builds
```yaml
name: MainPush

on:
  push:
    branches:
      - main
    paths-ignore:
      - 'README.md'
      - 'CHANGELOG.md'
      - '.github/**'
      - '.development/**'
  workflow_dispatch:
```

3. Add the below yaml to your release.yaml file in application repositories.

```yaml
name: Release

on:
  release:
    types: [published]

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - name: Get the VERSION
        shell: bash
        run: echo "VERSION=${GITHUB_REF#refs/tags/}" >> $GITHUB_ENV

      - name: Auto Release Notes
        if: ${{ success() }}
        uses: sdi-one-foundation/auto-release-notes-action@v1.1.1
        with:
          github-token: ${{ secrets.DEVOPS_BOT_TOKEN }}
          version: ${{ env.VERSION }}
          service-name: ${{ github.event.repository.name }}
          team-name: ${{ github.repository_owner }}
          ms-teams-webhook-url: ${{ secrets.MS_TEAMS_RELEASE_WEBHOOK_URL }}
```
