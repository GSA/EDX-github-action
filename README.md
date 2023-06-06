# GSA EDX GitHub Action

## Usage

### Basic

This action requires permission to post audit results as a pull request comment.

```yaml
permissions:
  pull-requests: write
```

```yaml
uses: GSA/EDX-github-action@v1
with:
  config-path: "./lighthouserc.js"
```

## Inputs

### `config-file`

**Optional** Path to Lighthouse configuration file.

## Outputs

None
