# Spatial organization of malignant and immune compartments in HNSCC

An independent spatial transcriptomics portfolio project using public 10x Genomics Visium data to describe malignant and immune organization across two HNSCC tissue sections.

This repository is designed as a transparent, end-to-end analysis record for bioinformatics engineers, computational biologists and technical recruiters. It connects documented R notebooks, saved analysis objects, reusable result tables, publication-style figures and a scientific report.

- [Read the full scientific report](https://fadwa7.github.io/spatial-hnscc-immune-niches/reports/HNSCC_spatial_transcriptomics_report.html)

## Background

Head and neck squamous cell carcinoma (HNSCC) contains malignant epithelial, stromal, vascular and immune compartments arranged within a heterogeneous tissue microenvironment. Standard 10x Genomics Visium assays retain spatial coordinates while measuring expression in spots that may contain multiple cells. Spot-level profiles therefore represent local transcriptional mixtures rather than single-cell measurements.

This project uses spatial expression patterns, transcriptome-based clustering and deconvolution to describe tissue organization. It does not infer direct cell-cell interaction, clinical response or causal biological mechanisms.

## Reference Study and Project Positioning

This independent portfolio project was inspired by the study:

> Adimi Y. et al. *A new spatial multi-omics approach to deeply
> characterize human cancer tissue using a single tissue section*.
> bioRxiv, 2024.  
> DOI: [10.1101/2024.10.03.616510](https://doi.org/10.1101/2024.10.03.616510)

The original study, conducted by researchers affiliated with Sanofi R&D
and academic clinical institutions, combined 10x Visium spatial
transcriptomics with Phenocycler spatial proteomics on head and neck
squamous cell carcinoma tissue sections.

This portfolio does not reproduce the complete multi-omics study.
It focuses exclusively on the publicly available ST-only Visium data
and develops an independent transcriptomics workflow covering:

- sample-level quality control and clustering;
- cross-sample RPCA integration;
- marker-based interpretation of spatial transcriptional domains;
- SpaCET deconvolution of malignant, stromal and immune compartments;
- sample-relative spatial immune-niche characterization.

The original manuscript, public dataset and companion repository were
used as scientific and methodological references. All interpretations
presented here were derived independently from the analyses documented
in this repository.

### Original resources

- [Preprint](https://doi.org/10.1101/2024.10.03.616510)
- [Public dataset on Zenodo](https://zenodo.org/records/13736222)
- [Companion analysis repository](https://github.com/EPL-repo/PP_ST_multi)


## Biological question

> **How are malignant and immune compartments spatially organized across two HNSCC tissue sections?**

The analysis focuses on spatial transcriptional domains, estimated malignant and immune contributions, and sample-relative immune niches. Distinct malignant transcriptional states were not formally characterized.

## Dataset

The project analyzes two biological HNSCC tissue sections from a public 10x Genomics Visium dataset. Each sample is processed independently before descriptive cross-sample integration and comparison.

| Sample | Platform | Input | Working clustering resolution | Analytical role |
|:--|:--|:--|:--:|:--|
| 17B5776 | 10x Genomics Visium | Filtered Space Ranger expression matrix, tissue image and spatial coordinates | 0.6 | Sample-level analysis, integration and SpaCET deconvolution |
| 19H1257 | 10x Genomics Visium | Filtered Space Ranger expression matrix, tissue image and spatial coordinates | 0.8 | Sample-level analysis, integration and SpaCET deconvolution |

The data are available from [Zenodo record 13736222](https://zenodo.org/records/13736222). Raw data are excluded from version control and are expected under the paths recorded in [`config/samples.tsv`](config/samples.tsv).

## Analysis overview

The analysis follows the existing notebook order:

1. Import filtered Space Ranger counts, tissue images and spatial coordinates.
2. Evaluate transcript, detected-feature and mitochondrial quality metrics in distributional and tissue space.
3. Apply the documented filters, normalize each sample with SCTransform, and construct PCA, neighbor-graph and UMAP representations.
4. Evaluate sample-level clustering resolutions and interpret spatial domains using marker genes and tissue localization.
5. Merge both samples and perform reciprocal PCA (RPCA) integration for descriptive joint clustering.
6. Apply SpaCET independently with the HNSC cancer model to estimate malignant, stromal and immune transcriptional fractions.
7. Derive a total immune fraction from major immune lineages and define Immune-low, Intermediate and Immune-high spots using quartiles calculated separately within each sample.
8. Summarize the relative immune-lineage composition of Immune-high niches and assemble portfolio figures, tables and reporting outputs.

Major and subordinate SpaCET hierarchy levels are not summed because they overlap. SpaCET outputs are estimated transcriptional fractions, not exact cell counts.

## Workflow diagram

```mermaid
flowchart TD
    A[Public 10x Visium data] --> B[Sample-level QC]
    B --> C[SCTransform normalization]
    C --> D[PCA and clustering]
    D --> E[Cross-sample RPCA integration]
    E --> F[Marker-based compartment interpretation]
    F --> G[SpaCET deconvolution]
    G --> H[Immune-fraction quantification]
    H --> I[Spatial immune-niche characterization]
    I --> J[Portfolio figures and scientific report]
```

## Repository structure

The tree below reflects the current repository layout. Local data, saved R objects, generated output and cache directories are omitted for clarity.

```text
.
├── README.md
├── LICENSE
├── _quarto.yml
├── index.qmd
├── methods.qmd
├── results.qmd
├── styles.scss
├── notebooks/
│   ├── 01_exploration_17B5776.Rmd
│   ├── 02_exploration_19H1257.Rmd
│   ├── 03_cross_sample_comparison.Rmd
│   └── 04_spacet_deconvolution.Rmd
├── renv/
│   ├── activate.R
│   └── settings.json
├── renv.lock
├── reports/
│   └── HNSCC_spatial_transcriptomics_report.Rmd
├── results/
│   ├── figures/                     # exported PNG figures
│   └── tables/                      # marker and SpaCET summary tables
└── spatial-hnscc-immune-niches.Rproj
```

## Notebook overview

| Notebook | Main tools | Purpose | Main outputs |
|:--|:--|:--|:--|
| [`01_exploration_17B5776.Rmd`](notebooks/01_exploration_17B5776.Rmd) | Seurat, SCTransform, ggplot2, clustree, hdf5r, here | Import and quality-control sample 17B5776; perform PCA, UMAP, clustering and marker-guided domain assessment | Clustered Seurat object at resolution 0.6, marker table and exploratory/spatial figures |
| [`02_exploration_19H1257.Rmd`](notebooks/02_exploration_19H1257.Rmd) | Seurat, SCTransform, ggplot2, clustree, hdf5r, here | Import and quality-control sample 19H1257; perform PCA, UMAP, clustering and marker assessment | Clustered Seurat object at resolution 0.8 and notebook-embedded exploratory results |
| [`03_cross_sample_comparison.Rmd`](notebooks/03_cross_sample_comparison.Rmd) | Seurat, RPCA integration, dplyr, ggplot2, patchwork, here | Harmonize metadata, merge both samples, create joint and integrated representations, identify markers and project integrated clusters into tissue space | Integrated resolution-0.6 Seurat object, integrated UMAP, spatial cluster maps and marker tables |
| [`04_spacet_deconvolution.Rmd`](notebooks/04_spacet_deconvolution.Rmd) | SpaCET, dplyr, tidyr, tibble, ggplot2, here | Estimate cellular transcriptional fractions, derive total immune fractions, classify sample-relative immune niches and summarize Immune-high composition | SpaCET objects, combined spot-level fractions, summary tables and immune-niche figures |

## Main results

The following are verified descriptive observations from the notebooks and saved results:

- Both sections contain spatially structured transcriptional heterogeneity at the Visium-spot level.
- Marker-guided analysis of 17B5776 identifies domains associated with immune, epithelial, stromal, hypoxic, inflammatory, interferon-responsive and proliferative programs. These are provisional mixed-spot annotations, not purified cell types.
- Most integrated clusters contain spots from both samples, while some are sample-enriched; integrated labels also form structured tissue domains.
- The saved SpaCET summaries show a higher median estimated malignant fraction in 19H1257 and a higher median derived immune fraction in 17B5776. These are section-level descriptions, not population-level effects.
- The median derived immune fractions are 0.318 for 17B5776 and 0.100 for 19H1257 after rounding to three decimal places.
- Immune-high niches contain mixed lineage-associated transcriptional signals. Neutrophils have the largest mean relative share in the saved Immune-high composition summaries for both samples.

Representative outputs are available in [`results/figures`](results/figures) and numerical summaries in [`results/tables`](results/tables).

## Reproducibility

- The four notebooks preserve the sequential computational record from sample-level preprocessing to niche characterization.
- Project-relative paths are handled with the `here` package.
- Fixed random seeds are specified for UMAP and clustering where documented.
- Intermediate RDS objects, marker tables, spot-level fractions and summary tables separate expensive computation from reporting.
- [`renv.lock`](renv.lock) records R 4.3.3 and the currently captured R dependencies.

The current `renv.lock` does not include the full analysis stack, notably Seurat and SpaCET. Exact package versions used for the completed analysis therefore require manual verification before full computational reproduction.

## Installation and usage

This is an R project, so it uses `renv` rather than a Python `venv`. Both serve the same purpose: isolating project dependencies from the user-level environment.

### Requirements

- R 4.3.3, as recorded in `renv.lock`
- R packages used in the notebooks, including Seurat, SpaCET, `ggplot2`, `dplyr`, `tidyr`, `tibble`, `patchwork`, `clustree`, `hdf5r` and `here`
- Filtered Space Ranger outputs for both samples at the paths specified in `config/samples.tsv`

### Install the project environment

Clone the repository, move into it and restore the isolated R environment:

```bash
git clone <repository-url>
cd spatial-hnscc-immune-niches
Rscript -e 'install.packages("renv", repos="https://cloud.r-project.org")'
Rscript -e 'renv::restore()'
```

The current lockfile does not yet contain Seurat and SpaCET. Install any missing analysis dependencies inside the project environment before running the complete workflow, then record them with `renv::snapshot()`.

### Prepare the input data

```text
data/raw/17B5776-1_STonly/17B5776-1-STonly/
data/raw/19h1257-1_STonly/19h1257-1_v3/
```

Each directory must contain the filtered expression matrix, tissue image files and spatial coordinates produced by Space Ranger.

### Run the analysis

Execute the notebooks from the repository root in their documented order:

```bash
Rscript -e "rmarkdown::render('notebooks/01_exploration_17B5776.Rmd')"
Rscript -e "rmarkdown::render('notebooks/02_exploration_19H1257.Rmd')"
Rscript -e "rmarkdown::render('notebooks/03_cross_sample_comparison.Rmd')"
Rscript -e "rmarkdown::render('notebooks/04_spacet_deconvolution.Rmd')"
```

The notebooks write intermediate objects and final summaries under `results/`. SCTransform, marker detection, RPCA integration and SpaCET deconvolution can be computationally expensive.

After the saved tables and figures are available, generate the scientific report with:

```bash
Rscript -e "rmarkdown::render('reports/HNSCC_spatial_transcriptomics_report.Rmd')"
```

## Project status

The exploratory analysis, two-sample RPCA integration, SpaCET deconvolution, immune-niche summaries, saved result tables, portfolio figures and scientific-report source are present. The project should be considered a documented portfolio analysis rather than a production pipeline or a population-level HNSCC study.

The complete analysis software environment still needs to be captured in `renv.lock`. The reported retained-spot count for 19H1257 also requires manual confirmation because the notebook refers to an apparently undefined `dim_after_qc` object.

## Limitations

- Only two biological samples are included; all cross-sample comparisons are descriptive.
- Spots within a tissue section are spatially correlated and are not independent biological replicates.
- A standard Visium spot may contain multiple cells, so clusters and niches represent mixed local transcriptional signals.
- Quality-control thresholds may remove genuine low-complexity tissue as well as technical failures.
- Cluster interpretation is marker-guided and provisional; a single marker does not establish cell identity.
- RPCA integration may retain technical structure or attenuate genuine sample-specific biology.
- SpaCET fractions depend on reference signatures and model assumptions and do not represent exact cell counts.
- Immune-high and Immune-low categories use sample-specific quartiles, so labels are relative within each section and are not directly equivalent across samples.
- Relative Immune-high composition is compositional and does not measure absolute abundance or enrichment against Immune-low spots.
- Distinct malignant transcriptional states were not formally characterized.
- No clinical, therapeutic or population-level conclusions can be drawn from this study design.

## Future directions

Future work should remain separate from the completed analysis and could include:

- adding more biological samples to support replicated, population-level spatial inference;
- formally characterizing malignant transcriptional states with an explicitly validated analysis strategy;
- validating spatial domains and immune niches against tissue histology or independent biological measurements;
- evaluating spatial autocorrelation and replicated spatial statistics where the expanded study design supports them;
- capturing the full analysis environment and adding automated reproducibility checks;
- extending the public documentation as the analysis develops.

## Author

**Fadwa EL KHADDAR**

Independent bioinformatics portfolio project based on public data.
