# auto-release-notes-action
A composite Github Action for automatic release notes and notifications.

Example release.yaml that uses this action:

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
        uses: sdi-one-foundation/auto-release-note-action@v1
        with:
          github-token: ${{ secrets.DEVOPS_BOT_TOKEN }}
          version: ${{ env.VERSION }}
          service-name: ${{ github.event.repository.name }}
          team-name: "Name of Team"
          ms-teams-webhook-url: ${{ secrets.MS_TEAMS_RELEASE_WEBHOOK_URL }}
```
