# R `pr-push` Action

> Pull request pushing

This GitHub Action pushes changes to the head of the pull request branch. It
assumes you have already commited any results prior to running. It is intended
to be used by actions which trigger based on comment creation. It is often
paired with the
[pr-fetch](https://github.com/r-lib/actions/tree/master/pr-fetch) action.

## Usage

Generally this is used to push back to a pull request associated with a
comment, you can filter the comments using an `if` clause, so it is only run for
specific things. You need to include your token to the action in order for it
to work, as it needs to query the GitHub API for the pull request information.

```yaml
on:
  issue_comment:
    types: [created]

jobs:
  foo_command:
    if: startsWith(github.event.comment.body, '/foo')
    name: foo
    runs-on: macOS-latest
    steps:
      - uses: actions/checkout@v1
      - uses: r-lib/actions/pr-fetch@master
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
      - run: |
        echo "hi" > foo
        git commit -m 'foo added' --author 'GitHub Actions <actions@github.com>'
      - uses: r-lib/actions/pr-push@master
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```