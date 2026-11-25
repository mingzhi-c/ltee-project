# LTEE variant calling

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24535491.svg)](https://doi.org/10.5281/zenodo.24535491)

Variant calling on three *E. coli* clones from population Ara-3 of Richard
Lenski's Long-Term Evolution Experiment (LTEE), frozen at 5,000, 15,000 and
50,000 generations. Question: how many mutations accumulated, and is the
answer robust to the minimum read depth used to filter variant calls?

Built as the example project of the Reproducible and Open Science lessons.
The analysis steps follow the Data Carpentry lesson *Wrangling
Genomics* (CC BY 4.0).

Author: Mingzhi Chen (ORCID [0009-0005-2945-1547](https://orcid.org/0009-0005-2945-1547))

## Publication

None yet. The data and biology are from Tenaillon et al. (2016), *Nature*
536:165–170, <https://doi.org/10.1038/nature18959>.

## Reproduce the results

Requirements: [Pixi](https://pixi.prefix.dev/latest/) and an internet
connection. Linux or macOS (on Windows use WSL2: `bwa`, `samtools` and
`bcftools` have no Windows builds). No other installation is needed.

```bash
git clone https://github.com/mingzhi-c/ltee-project.git
cd ltee-project
pixi run reproduce
```

`pixi run reproduce` installs the exact software versions in `pixi.lock`,
downloads the raw data and checks its checksums, runs the Snakemake workflow
and renders the report. A GitHub Action runs the same command on every push.

`pixi run compare-to-shell` compares Snakemake with the original shell script.

Other entry points: `pixi run run-nextflow` (the same pipeline in Nextflow),
`pixi run compare-workflows` (checks that both give identical variant calls),
`pixi task list` (all tasks).

A container image of the pipeline environment can be built from
`containers/Dockerfile`; see the [container guide](docs/containers.md) for
building, running and archiving images, including the published image digest.

## Hardware

Measured on 2026-11-23 on an Apple M5 Pro MacBook Pro with 24 GB RAM
and 15 logical CPUs. The Pixi environments use about 3.7 GB of disk
(1.1 GB default, 2.6 GB pipeline); raw data uses 332 MB. The current
results directory uses about 1.0 GB, including shell-script, Snakemake and
Nextflow outputs. Allow extra disk space for workflow caches and container
images. The Snakemake task uses up to 4 cores.

## Outputs

| Path | Content |
|------|---------|
| `reports/ltee-report.html` | The report: data, read quality, variants, the depth sensitivity analysis, software versions |
| `results/qc/multiqc/multiqc_report.html` | Read quality summary |
| `results/vcf/` | Filtered variant calls per sample (minimum depth from `config/config.yaml`) |
| `results/sensitivity/` | Variant calls at each depth in `sensitivity_depths` |
| `results/stats/` | Alignment (`flagstat`) and variant (`bcftools stats`) statistics |

Expected filtered variants at minimum depth 2: SRR2589044 10, SRR2584863 25,
SRR2584866 775.

## Data

Raw reads: a subset of three runs prepared by Data Carpentry, figshare,
<https://doi.org/10.6084/m9.figshare.7726454> (file `sub.tar.gz`, CC0).
Original runs in NCBI BioProject PRJNA295606. Reference: *E. coli* B str.
REL606, GenBank CP000819.1. Sources, provider checksums and our checksums are
in `metadata/`; the data is not stored in this repository.

## Repository contents

See the [folder guide](docs/structure.md), [workflow diagram](docs/rulegraph.svg)
and [software versions](docs/software-versions.txt). Each folder also has its own README.

## Cite

See `CITATION.cff` (GitHub: "Cite this repository").

## License

Code: MIT (`LICENSE`). Report text and figures: CC BY 4.0. The read subset is
CC0 (see Data).

## Contact

Questions and problems: open an issue on GitHub. Contributions: see
`CONTRIBUTING.md`. Everyone taking part follows `CODE_OF_CONDUCT.md`.

Project layout generated from the [CBIO7501 template](https://github.com/khan-lab/cbio7501-f26/tree/main/templates/cookiecutter-repro-project).
