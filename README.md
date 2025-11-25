# Behavioral Task Battery Data Analysis

MATLAB-based statistical analysis pipeline for behavioral task experiments with rodent models, focusing on OPRM1 variants and pharmacological interventions.

## Overview

This project analyzes behavioral data from multiple experimental paradigms:
- **Long Evans Rats**: Baseline behavioral task performance
- **10xOPRM1 Rats**: OPRM1 variant model analysis
- **2xOPRM1 Rats**: DREADD-based chemogenetic manipulation studies

The analysis pipeline includes normalization, visualization, and statistical testing (ANOVA, t-tests, non-parametric tests).

## Project Structure

```
├── runme.m                          # Main analysis script
├── Data/                            # Experimental data (CSV files)
│   ├── LongEvans/                   # Long Evans rat experiments
│   ├── 10xOPRM1/                    # 10x OPRM1 variant experiments
│   ├── 2xOPRM1/                     # 2x OPRM1 + DREADD experiments
│   └── FoodConsumption/             # Food consumption assays
└── [Analysis Scripts]               # Statistical and plotting functions
```

## Core Analysis Scripts

### Main Script
- **`runme.m`**: Primary analysis pipeline
  - Runs 2-way ANOVAs for all experimental groups
  - Generates publication-ready figures
  - Performs post-hoc comparisons

### Statistical Functions
- **`paired_ttest.m`**: Paired t-test and Wilcoxon signed-rank test
- **`unpaired_ttest.m`**: Welch's t-test and Wilcoxon rank-sum test (Mann-Whitney U)
- **`wilcoxon_rs_results.m`**: Wilcoxon rank-sum test implementation

### Data Processing
- **`buildLongTable2way.m`**: Creates long-format tables for 2-way ANOVA (Group × Task)
- **`buildLongTable3way.m`**: Creates long-format tables for 3-way designs (Group × DREADD × Task)
- **`buildLongTable1way.m`**: Single-factor table builder
- **`buildLongTableCombined3way.m`**: Combined 3-way analysis across files

### Visualization
- **`barPlotWithPoints.m`**: Bar plots with individual data points overlaid

## Experimental Tasks

### Simple Tasks
- **Food Alone (FA)**: Food-seeking behavior without additional stimuli
- **Toy Alone (TA)**: Object interaction task
- **Light Alone (LA)**: Light-cued behavior

### Complex Tasks
- **Food + Light (FL)**: Food-seeking with light cue
- **Toy + Light (TL)**: Object interaction with light cue

## Data Format

CSV files should contain:
- Subject identifiers
- Treatment groups (e.g., Saline, Ghrelin)
- Behavioral measurements (frequency, time, consumption)
- For DREADD experiments: genotype information (WT, Inhibitory, Excitatory)

## Usage

### Quick Start
```matlab
% Run complete analysis pipeline
runme
```

### Individual Analyses
```matlab
% Paired t-test on specific dataset
paired_ttest('Data/LongEvans/Food Center Freq_K.csv');

% Unpaired t-test
unpaired_ttest('Data/10xOPRM1/Food Alone 10x.csv');

% 2-way ANOVA with visualization
longTbl = buildLongTable2way(3, false, '', ...
    'Data/LongEvans/Food Center Freq_K.csv', ...
    'Data/LongEvans/Toy Alone Freq_K.csv', ...
    'Data/LongEvans/Light Alone Freq_K.csv');
barPlotWithPoints(longTbl, 'Task', 'Group', 'Normalized Frequency', 'Simple task');
[p, tbl, stats] = anovan(longTbl.Y, {longTbl.Group, longTbl.Task}, ...
    'model', 'interaction', 'varnames', {'Group','Task'});
```

## Normalization Options

The `buildLongTable` functions support multiple normalization types (specified as first parameter):
- **Type 1**: Raw values
- **Type 2**: Z-score normalization
- **Type 3**: min-max normalization

## Statistical Approach

- **Parametric**: t-tests (paired/unpaired), ANOVA with post-hoc comparisons
- **Non-parametric**: Wilcoxon signed-rank, Wilcoxon rank-sum (Mann-Whitney U)
- **Multiple comparisons**: Tukey-Kramer for ANOVA post-hoc tests

## Requirements

- MATLAB R2019b or later
- Statistics and Machine Learning Toolbox

## Notes

- Data files in `Data/` directory are excluded from version control
- Output figures and MAT files are also gitignored
- All analyses use unequal variance corrections where appropriate (Welch's t-test)
---

*For questions or issues, refer to commented sections in `runme.m` for specific analysis workflows.*
