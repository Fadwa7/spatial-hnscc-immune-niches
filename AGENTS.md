# Spatial HNSCC project instructions

## Project scope

This repository contains a spatial transcriptomics portfolio project
investigating malignant and immune spatial organization in two HNSCC
Visium samples.

The notebooks must remain understandable to bioinformatics engineers,
scientists and technical recruiters.

## Notebooks in scope

- notebooks/01_exploration_17B5776.Rmd
- notebooks/02_exploration_19H1257.Rmd
- notebooks/03_cross_sample_comparison.Rmd
- notebooks/04_spacet_deconvolution.Rmd

## Documentation rules

- Write all Markdown explanations and code comments in English.
- Preserve the existing analysis order.
- Preserve all object names, parameters, thresholds and output paths.
- Do not change executable R code unless an actual error is identified.
- Do not add new analyses.
- Do not remove existing analyses or results.
- Do not rerun computationally expensive analyses.
- Do not knit the notebooks unless explicitly requested.
- Do not invent numerical results or biological conclusions.
- Distinguish observations from hypotheses and interpretations.
- Add comments only when the purpose of the code is not obvious.
- Avoid commenting every individual line.
- Prefer clear Markdown sections over excessive inline comments.
- Keep explanations concise but sufficiently informative.

## Required notebook structure

For each major analysis section, add:

1. A concise biological or methodological objective.
2. A brief explanation of the input data.
3. An explanation of the main processing step.
4. A description of the expected output.
5. A short interpretation after important tables or figures.
6. Relevant methodological limitations.

## Scientific terminology

- Use "spot" for standard Visium observations, not "bin".
- A Visium spot may contain multiple cells.
- SpaCET outputs estimated transcriptional fractions, not exact cell counts.
- Immune-high and Immune-low labels are sample-relative categories.
- Do not interpret spot-level observations as independent biological
  replicates.
- Do not make clinical or therapeutic claims.
- The project currently contains two biological samples, so cross-sample
  comparisons are descriptive.

## SpaCET-specific requirements

Explain clearly:

- how SpaCET estimates malignant, stromal and immune fractions;
- why major and subordinate cell lineages must not be added together;
- how the total immune fraction was calculated;
- why only major immune lineages were included;
- how sample-specific quartiles define Immune-low, Intermediate and
  Immune-high spots;
- why an Immune-high spot from one sample is not necessarily equivalent
  to an Immune-high spot from another sample;
- how the relative composition of Immune-high niches was calculated.

## Validation

After editing:

- Review the Git diff.
- Confirm that executable R code has not been changed.
- Report any suspected methodological or coding error without correcting
  it automatically.
- List all edited files and summarize the documentation added.
