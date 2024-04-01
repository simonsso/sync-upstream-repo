# Sync Upstream Repo Fork

There are other flavours of this Github Action

This one favours a hard tracking policy, it will overwrite anything on the local branch with the contents of the remote

## Use case

- Perserve a repo while keeping up-to-date (rather than to clone it).
- Have a branch in sync with upstream, and pull changes into dev branch.

## Usage


```YAML
name: Sync Upstream

env:
  # Required, URL to upstream (fork base)
  UPSTREAM_REPO: "https://github.com/dabreadman/go-web-proxy.git"
  # Required, token to authenticate bot, could use ${{ secrets.GITHUB_TOKEN }} 
  # Over here, we use a PAT instead to authenticate workflow file changes.
  WORKFLOW_TOKEN: ${{ secrets.WORKFLOW_TOKEN }}
  # Optional, defaults to master
  UPSTREAM_BRANCH: "master"
  # Optional, defaults to UPSTREAM_BRANCH
  DOWNSTREAM_BRANCH: ""
  # Optional fetch arguments
  FETCH_ARGS: ""
  # Optional push arguments
  PUSH_ARGS: ""

# This runs every day on 1801 UTC
on:
  schedule:
    - cron: '1 18 * * *'
  # Allows manual workflow run (must in default branch to work)
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: GitHub Sync to Upstream Repository
        uses: simonsso/sync-upstream-repo@v1
        with: 
          upstream_repo: ${{ env.UPSTREAM_REPO }}
          upstream_branch: ${{ env.UPSTREAM_BRANCH }}
          downstream_branch: ${{ env.DOWNSTREAM_BRANCH }}
          token: ${{ env.WORKFLOW_TOKEN }}
          fetch_args: ${{ env.FETCH_ARGS }}
          push_args: ${{ env.PUSH_ARGS }}

```