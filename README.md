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

| Input                  | Description                                       | Required | Default     |
| ---------------------- | ------------------------------------------------- | -------- | ----------- |
| `input_dir`            | Directory containing Java source files            | No       | `./src`     |
| `metrics`              | Comma-separated list of metrics (e.g., loc,cc,mi) | No       | all of them |
| `output_dir`           | Output directory for visualization results        | No       | `.visual`   |
| `theme`                | Visualization theme (light or dark)               | No       | `light`     |
| `color_palette`        | Color palette (eg: puBu, magma, oranges)          | No       | `puBu`      |
| `treemap_algorithm`    | The treemap algorithm to be used                  | No       | `squarify`  |
| `map_width`            | The city map width                                | No       | `1000`      |
| `map_depth`            | The city map depth                                | No       | `1060`      |
| `show_legend`          | Whether to show the legend or not                 | No       | `true`      |
| `pmd_version`          | PMD version to use                                | No       | `7.13.0`    |
| `feather_city_version` | Feather City version to use                       | No       | latest one  |

## Important Notes

### PMD Configuration

The action automatically installs PMD and configures it for use with Feather City. No additional setup is required.

### Available Metrics

The following metrics are available for Java projects:

| ID     | Name                       | Description                                        |
| ------ | -------------------------- | -------------------------------------------------- |
| `loc`  | Lines of Code              | Number of lines of code                            |
| `cc`   | Cyclomatic Complexity      | Measure of code complexity based on control flow   |
| `iss`  | Issues Count               | Number of issues detected by static analysis       |
| `cloc` | Comment Lines              | Number of lines containing comments                |
| `nom`  | Number of Methods          | Number of methods or functions                     |
| `wmc`  | Weighted Methods per Class | Sum of the complexities of the methods for a class |
| `stat` | Statements Count           | Number of statements in the class                  |
| `ce`   | Efferent Coupling          | Number of components that the class depends on     |
| `ca`   | Afferent Coupling          | Number of components that depend on the class      |

### Color Palette

The color palettes are taken from [d3 sequential schemes](https://d3js.org/d3-scale-chromatic/sequential) and can be copied from there. Some recommended examples are:
- [puBu](https://d3js.org/d3-scale-chromatic/sequential#interpolatePuBu)
- [oranges](https://d3js.org/d3-scale-chromatic/sequential#interpolateOranges)
- [inferno](https://d3js.org/d3-scale-chromatic/sequential#interpolateInferno)
- [magma](https://d3js.org/d3-scale-chromatic/sequential#interpolateMagma)
- [rdPu](https://d3js.org/d3-scale-chromatic/sequential#interpolateRdPu)

For accessibility reasons, we suggest you to use color palettes that are also distinguishable for people with visual impairments.
## License

[MIT License](LICENSE)