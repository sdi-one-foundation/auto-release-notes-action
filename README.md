# auto-release-notes-action
A composite Github Action for automatic release notes and notifications.

A global tag will be created (see release.yml) on every release.  This global tag will have the format of "v\<major version\>" (example: v1).  This global tag will point to the most recent release.  The input needs to be change upon every major version change (v1 to v2).

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
        uses: sdi-one-foundation/auto-release-noteS-action@v1
        with:
          github-token: ${{ secrets.DEVOPS_BOT_TOKEN }}
          version: ${{ env.VERSION }}
          service-name: ${{ github.event.repository.name }}
          team-name: ${{ github.repository_owner }}
          ms-teams-webhook-url: ${{ secrets.MS_TEAMS_RELEASE_WEBHOOK_URL }}
```
