# Pull Request Branch Name

A github action that retrieves the pull request branch name and sets it in the output for other actions to use.

# Usage

```yaml
- uses: mdecoleman/pr-branch-name@1.2.0
  id: vars
  with:
    repo-token: ${{ secrets.GITHUB_TOKEN }}
- run: echo ${{ steps.vars.outputs.branch }}
```

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
