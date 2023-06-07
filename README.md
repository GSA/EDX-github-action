# GSA EDX GitHub Action

> GitHub Action to run GSA EDX Lighthouse plugin

This composite action integrates Lighthouse CI and the GSA EDX Lighthouse plugin with Github Actions environment. The results of the audit are added as a pull request comment.

## Example

Run Lighthouse on each pull request.

Create `.github/workflows/main.yml` and include the `EDX-github-action` step.

```yml
name: Lighthouse CI

on: [pull_request]

permissions:
  pull-requests: write

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          cache: "npm"
      - run: npm ci
      - name: Audit site using Lighthouse
        uses: GSA/EDX-github-action@v1
```

Make a `lighthouserc.json` file with [LHCI assertion syntax](https://github.com/GoogleChrome/lighthouse-ci/blob/master/docs/configuration.md).

#### lighthouserc.json

```json
{
  "ci": {
    "collect": {
      "settings": {
        "plugins": ["lighthouse-plugin-edx"]
      },
      "startServerCommand": "NODE_ENV=production npm run start",
      "url": ["http://localhost:3000"]
    }
  }
}
```

## Recipes

<details>
 <summary>Use a directory of static files instead of a URL</summary><br>

Create `.github/workflows/main.yml` and include the `EDX-github-action` step.

#### main.yml

```yml
name: Lighthouse CI

on: [pull_request]

permissions:
  pull-requests: write

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          cache: "npm"
      - run: npm ci
      - run: npm run build
      - name: Audit site using Lighthouse
        uses: GSA/EDX-github-action@v1
```

Make a `lighthouserc.json` file with [LHCI assertion syntax](https://github.com/GoogleChrome/lighthouse-ci/blob/master/docs/configuration.md).

#### lighthouserc.json

```json
{
  "ci": {
    "collect": {
      "settings": {
        "plugins": ["lighthouse-plugin-edx"]
      },
      "staticDistDir": "./_site"
    }
  }
}
```

</details>

<details>
 <summary>Integrate with a Jekyll based site</summary><br>

Create `.github/workflows/main.yml` and include the `EDX-github-action` step.

#### main.yml

```yml
name: Lighthouse CI

on: [pull_request]

permissions:
  pull-requests: write

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: "2.7"
          bundler-cache: true
      - name: Audit site using Lighthouse
        uses: GSA/EDX-github-action@v1
```

Make a `lighthouserc.json` file with [LHCI assertion syntax](https://github.com/GoogleChrome/lighthouse-ci/blob/master/docs/configuration.md).

#### lighthouserc.json

```json
{
  "ci": {
    "collect": {
      "settings": {
        "plugins": ["lighthouse-plugin-edx"]
      },
      "startServerCommand": "bundle exec jekyll serve",
      "url": ["http://localhost:4000"]
    }
  }
}
```

</details>

<details>
 <summary>Use a configuration file in a custom location</summary><br>

Create `.github/workflows/main.yml` and include the `EDX-github-action` step.

#### main.yml

```yml
name: Lighthouse CI

on: [pull_request]

permissions:
  pull-requests: write

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Audit site using Lighthouse
        uses: GSA/EDX-github-action@v1
        with:
          config-path: "./some/dir/lighthouserc.json"
```

Make a `lighthouserc.json` file with [LHCI assertion syntax](https://github.com/GoogleChrome/lighthouse-ci/blob/master/docs/configuration.md).

#### lighthouserc.json

```json
{
  "ci": {
    "collect": {
      "settings": {
        "plugins": ["lighthouse-plugin-edx"]
      },
      "url": ["http://localhost:4000"]
    }
  }
}
```

</details>

## Inputs

### `config-path`

**Optional** Path to Lighthouse configuration file.

## Outputs

See https://github.com/treosh/lighthouse-ci-action/blob/main/README.md#outputs
