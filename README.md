# Feather City Analysis Action

This GitHub Action analyzes Java projects using Feather City and generates HTML visualizations.

## Features

- Analyzes Java source code with various metrics (Lines of Code, Cyclomatic Complexity, PMD Issues, etc.)
- Generates HTML visualizations for better code understanding
- Customizable themes and metrics
- Automatically commits visualization results to your repository (optional)

## Usage

### Basic Usage

Add this to your GitHub workflow file (e.g., `.github/workflows/feather-city.yml`):

```yaml
name: Analyze with Feather City
on:
  push:
    branches: [ main ]
  workflow_dispatch:  # Allow manual triggering

permissions:
  contents: write  # Required if commit_results is true

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Feather City Analysis
        uses: livcristi/feather-city-action@v1
```

### Advanced Usage

You can customize the analysis by specifying input parameters:

```yaml
name: Analyze with Feather City
on:
  push:
    branches: [ main ]
  workflow_dispatch:
    inputs:
      metrics:
        description: "Metrics to analyze"
        required: false
        default: "loc,cc,cloc"

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Feather City Analysis
        uses: livcristi/feather-city-action@v1
        with:
          input_dir: './src/main/java'
          metrics: ${{ github.event.inputs.metrics || 'loc,cc,cloc' }}
          theme: 'dark'
          output_dir: './feather-city-report'
          commit_results: 'true'
```

## Inputs

| Input           | Description                                     | Required | Default    |
|----------------|-------------------------------------------------|----------|------------|
| `input_dir`     | Directory containing Java source files          | No       | `./src`    |
| `metrics`       | Comma-separated list of metrics (e.g., loc,cc,mi) | No     | `loc,cc,cloc`|
| `theme`         | Visualization theme (light or dark)             | No       | `light`    |
| `output_dir`    | Output directory for visualization results      | No       | `.visual`  |
| `commit_results`| Whether to commit results back to the repository| No       | `true`     |

## Important Notes

### Commit Permissions

If you set `commit_results: 'true'` (the default), your workflow needs permission to write to the repository:

```yaml
permissions:
  contents: write
```

Without this permission, the action won't be able to commit the generated visualization files back to your repository.

### PMD Configuration

The action automatically installs PMD and configures it for use with Feather City. No additional setup is required.

### Pull Request Branches

When running this action on pull requests, be aware that the commits will be made to the PR branch, not to the target branch.

## License

[MIT License](LICENSE)