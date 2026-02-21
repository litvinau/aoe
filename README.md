# aoe

A GitHub Action that runs [opencode](https://opencode.ai) to assist with software engineering tasks directly in your repository.

## Usage

This action runs opencode when triggered by issue comments or PR review comments containing `/oc` or `/opencode`.

### Example Workflow

```yaml
name: opencode

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

jobs:
  opencode:
    if: |
      contains(github.event.comment.body, ' /oc') ||
      startsWith(github.event.comment.body, '/oc') ||
      contains(github.event.comment.body, ' /opencode') ||
      startsWith(github.event.comment.body, '/opencode')
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: write
      pull-requests: write
      issues: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6
        with:
          persist-credentials: false

      - name: Run opencode
        uses: anomalyco/opencode/github@latest
        env:
          OPENCODE_API_KEY: ${{ secrets.OPENCODE_API_KEY }}
        with:
          model: opencode/minimax-m2.5-free
```

## Configuration

### Required Secrets

- `OPENCODE_API_KEY`: Your opencode API key

### Inputs

- `model`: The model to use (default: `opencode/minimax-m2.5-free`)

## Triggering opencode

Add a comment to an issue or PR with `/oc` followed by your request:

```
/oc Explain this code
```

or

```
/opencode Write tests for this function
```
