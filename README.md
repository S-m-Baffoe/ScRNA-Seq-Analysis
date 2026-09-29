# ScRNA-Seq Analysis with Scanpy

A HackBio learning notebook for exploring single-cell RNA-sequencing data with Python and Scanpy. The analysis uses a human–mouse mixture from 10x Genomics and covers quality-control plots, normalization, variable-gene selection, dimensionality reduction, clustering, and marker-gene ranking.

## Contents

- [hackbio-team-biobosses-smb.ipynb](hackbio-team-biobosses-smb.ipynb): the complete notebook, including saved code, figures, and outputs.
- `README.md`: data requirements, workflow, and reproducibility notes.

The raw dataset is downloaded by the notebook and is not stored in this repository.

## Data

The executable download cell retrieves the 10x Genomics archive named `10k_hgmm_3p_nextgem_Chromium_X_filtered_feature_bc_matrix.tar.gz` and extracts it beneath `dataset/`. The notebook reads the filtered matrix from:

```text
dataset/filtered_feature_bc_matrix/
```

This directory must contain the matching Matrix Market counts, feature annotations, and cell barcodes expected by `sc.read_10x_mtx`.

**Dataset identity:** the introductory text links to a 20k human–mouse mixture page, whereas the actual download command names a 10k dataset. Use the executable download cell to identify the data analyzed in the saved run. That run reports 9,545 cells and 68,886 features before variable-gene selection; these are recorded outputs, not newly verified counts.

## Getting started

1. Clone or download the repository and open `hackbio-team-biobosses-smb.ipynb` in Jupyter or a compatible hosted notebook environment.
2. Review the dataset-download and package-installation cells before executing them.
3. Provide Python with Scanpy, NumPy, pandas, Matplotlib, and the Leiden clustering dependencies (`leidenalg` and igraph), plus a notebook interface.
4. Run cells in order from the repository directory. The notebook uses shell commands such as `wget`, `tar`, and `mkdir`; those cells need a compatible shell or equivalent manual setup.
5. Ensure the `dataset/` and `write/` directories are available. Repeated `mkdir` commands may report that the folders already exist.
6. Review the scientific and compatibility notes below before interpreting results or adapting the workflow to new data.

The saved environment reports **Scanpy 1.8.1, AnnData 0.7.6, NumPy 1.19.5, pandas 1.3.3, and SciPy 1.7.1**. These versions describe the original run rather than a tested installation recipe. No environment lockfile is included, and older Scanpy calls may require adaptation in a newer environment.

## Analysis workflow

1. Download a filtered 10x expression matrix and load it into an AnnData object.
2. Make gene names unique and inspect highly expressed genes.
3. Calculate and visualize QC metrics, including counts and a mitochondrial-gene annotation.
4. Normalize counts and identify variable genes.
5. Subset variable genes, scale expression, and compute PCA and a nearest-neighbor graph.
6. Generate UMAP coordinates and Leiden clusters with resolution `0.5`.
7. Rank cluster-associated genes using the Wilcoxon method and display dot plots and heatmaps.
8. Compute t-SNE and save the analyzed AnnData object.

## Outputs

The notebook displays QC plots, expression plots, UMAP/t-SNE embeddings, and cluster-marker visualizations. Its final save step writes:

```text
write/human_mouse.h5ad
```

The saved run shows 10,288 selected variable genes and 10 Leiden clusters. These depend on the data, parameters, software versions, and preprocessing choices; they are not guaranteed results for other datasets.

## Interpretation and reproducibility notes

- **Mitochondrial annotation needs review.** The notebook checks for gene names beginning with `MT-`, but the loaded names include species prefixes such as `GRCh38_MT-CO3` and `mm10___mt-Co3`. The existing check can miss mitochondrial genes and make the resulting QC metric misleading.
- **Marker analysis needs review.** The saved marker-ranking step reports an overflow warning. Check normalization, log transformation, and the expression representation used for ranking before interpreting fold changes or significance.
- **Memory use can be substantial.** The recorded scaling step converts sparse data to a dense representation.
- **This is cluster-marker exploration.** The notebook does not establish a replicated treatment-versus-control differential-expression analysis.
- **New datasets require adaptation.** Update the input path, species/gene naming, QC choices, and downstream parameters rather than assuming the example settings are universal.

This README documents the existing notebook and its saved outputs. The analysis was not rerun and the code was not changed as part of the documentation update.

## Acknowledgments

The notebook credits 10x Genomics for the data and the [Scanpy PBMC tutorial](https://scanpy-tutorials.readthedocs.io/en/latest/pbmc3k.html) as a learning resource. No repository license file is currently included.
