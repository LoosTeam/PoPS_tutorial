# Tutorial for running PoPS on Esrum
PoPS is a gene prioritization method that can be applied to GWAS summary statistics. You can find the official Github [here](https://github.com/FinucaneLab/pops), and the publication [here](https://doi.org/10.1038/s41588-023-01443-6).

PoPS has been made availble as an environment module in the `loos_group-AUDIT` folder on Esrum. This tutorial describes how you can load PoPS and associated tools, and run the program from start to finish.

> ✏️ **_NOTE:_** Please note that you need to be added to the `loos_group-AUDIT` folder on [Esrum](https://cbmr-data.github.io/esrum/) to be able to access and run this tool as demonstrated here. If you have not already been added to this folder, then [contact me](#contact-for-help) for help. 

The data used for this tutorial is a set of publicly available summary statistics for schizophrenia, same as that used in the paper. PoPS is run in 3 steps - 

- [Step 0: Munge features](#2-run-step-0-munge-features) - `munge_feature_directory.py` accepts a directory of feature files and processes them into a more efficient format for downstream usage.
- [Step 1: Generate MAGMA scores](#3-run-step-1-generate-magma-scores) - MAGMA analysis is performed in 2 steps, **annotation** and **gene analysis**, to generate gene-level association statistics that are used as input for PoPS. You can find the documentation [here](https://cncr.nl/research/magma/).
- [Step 2: Run PoPS](#4-run-step-2-run-pops) - `pops.py` is run using the MAGMA analysis output, and the processed feature files to generate gene priority scores.

## 1. Loading modules
The modules PoPS and MAGMA need to be loaded to be able to access them. It is good practice to `purge` all loaded modules to avoid clashes in versions when loading new modules.

```
module purge
module use --prepend /projects/loos_group-AUDIT/apps/modules/modulefiles/
module load PoPS/v0.2
module load MAGMA/v1.10 
```
Loading these modules will give you access to the different scripts used for PoPS. You can check if it is correctly loaded with these commands: 
```
munge_feature_directory.py -h
pops.py -h
magma
```

## 2. Run step 0: Munge features
```
munge_feature_directory.py \
 --gene_annot_path example_data/gene_annot_jun10.txt \
 --save_prefix outputs/pops_features \
 --max_cols 500
```
| Flag | Description |
|-|-|
| --gene_annot_path | Path to gene annotation table. For the purposes of this script, only require that there is an ENSGID column |
| --feature_dir | Directory where raw feature files live. Each feature file must be a tab-separated file with a header for column names and the first column must be the ENSGID. Will process every file in the directory so make sure every file is a feature file and there are no hidden files. Please also make sure the column names are unique across all feature files. The easiest way to ensure this is to prefix every column with the filename. ✏️ **_NOTE:_** Here the default features are those used in the manuscript, find link [here](https://github.com/FinucaneLab/pops/issues/7). |
| --nan_policy | What to do if a feature file is missing ENSGIDs that are in gene_annot_path. Takes the values "raise" (raise an error), "ignore" (ignore and write out with nans), "mean" (impute the mean of the feature), and "zero" (impute 0). Default is "raise" |
| --save_prefix | Prefix to the output path. For each chunk i, 2 files will be written: {save_prefix}_mat.{i}.npy, {save_prefix}_cols.{i}.txt. Furthermore, row data will be written to {save_prefix}_rows.txt |
| --max_cols | Maximum number of columns per output chunk. Default is 5000 |

## 3. Run step 1: Generate MAGMA scores
The first MAGMA step generates 
### 3.1 MAGMA `annotation`
```
magma \
--annotate \
--snp-loc [SNPLOC_FILE] \
--gene-loc [GENELOC_FILE] \
--out [OUTPUT_PREFIX]
```

### 3.1 MAGMA `gene analysis`
```
magma \
--bfile {PATH_TO_REFERENCE_PANEL_PLINK} \
--gene-annot {PATH_TO_MAGMA_ANNOT}.genes.annot \
--pval {PATH_TO_SUMSTATS}.sumstats ncol=N \
--gene-model snp-wise=mean \
--out {OUTPUT_PREFIX}
```

## 4. Run step 2: Run PoPS

# Contact for help
Siddhi Jain
siddhi.jain@sund.ku.dk