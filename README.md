# Patchwork AutoFix Action

This GitHub Action uses [patchwork-cli](https://docs.patched.codes/patchwork/quickstart) to automatically fix code vulnerabilities in your repository using LLMs.

## Features

- Automatically detects vulnerabilities using Semgrep
- Generates fixes using LLMs (OpenAI, local models, or custom endpoints)
- Creates pull requests with the fixes
- Configurable severity and compatibility thresholds
- Supports custom branch naming and PR behavior

## Usage

```yaml
name: Security AutoFix
on:
  schedule:
    - cron: "0 0 1 * *" # Run once a month
  workflow_dispatch: # Allow manual triggers

jobs:
  autofix:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: patched-codes/AutoFix@0.1.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          openai_api_key: ${{ secrets.OPENAI_API_KEY }}
```

## Inputs

### Required

One of the following is required:

- `patched_api_key`: Patched API key for using Patched services ([Get an API key](https://app.patched.codes/api-keys))
- `openai_api_key`: OpenAI API key for the LLM ([Get an API key](https://platform.openai.com/account/api-keys))

### Optional

- `github_token`: GitHub token for creating pull requests (default: [Automatically set by GitHub](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication))
- `model`: LLM model to use (default: gpt-3.5-turbo)
- `client_base_url`: Base URL for the LLM API (default: https://api.openai.com/v1)
- `vulnerability_limit`: Maximum number of vulnerabilities to fix (default: 10, -1 for no limit)
- `severity`: Minimum severity level (unknown/note/info/warning/low/medium/error/high/critical)
- `compatibility`: Minimum compatibility threshold (unknown/low/medium/high)
- `branch_prefix`: Prefix for the created branch
- `disable_branch`: Disable creating new branches
- `disable_pr`: Disable creating pull requests
- `force_pr_creation`: Force push commits to existing PR

## Examples

### Basic Usage

```yaml
- uses: patched-codes/AutoFix@0.1.0
  with:
    patched_api_key: ${{ secrets.PATCHED_API_KEY }}
```

### Using Custom Model

```yaml
- uses: patched-codes/AutoFix@0.1.0
  with:
    openai_api_key: ${{ secrets.OPENAI_API_KEY }}
    model: "gpt-4"
    client_base_url: "https://api.openai.com/v1"
```

### High Severity Only

```yaml
- uses: patched-codes/AutoFix@0.1.0
  with:
    patched_api_key: ${{ secrets.PATCHED_API_KEY }}
    severity: "high"
    vulnerability_limit: 5
```

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
