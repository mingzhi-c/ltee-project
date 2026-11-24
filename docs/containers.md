# Container guide

Build the pipeline environment from `pixi.lock`, from the project folder:

```bash
docker build --platform linux/amd64 -f containers/Dockerfile \
  --build-arg ENV_NAME=pipeline -t ltee-project:0.1.0 .
```

The image contains the software. Mount the project at `/work` to make its
scripts and data available, then run the analysis:

```bash
docker run --rm --platform linux/amd64 -v "$PWD":/work -w /work ltee-project:0.1.0 \
  python scripts/download_data.py
docker run --rm --platform linux/amd64 -v "$PWD":/work -w /work ltee-project:0.1.0 \
  bash scripts/run_variant_calling.sh
```

Keep `--platform linux/amd64` on Apple silicon: the lock file includes the
Linux x86-64 packages used by this image. Results are written to `results/`
in the mounted project folder.

Checked on 2026-11-16 with samtools 1.25, FastQC 0.13.0 and BWA 0.7.19.
The final variant counts are 25 (SRR2584863), 775 (SRR2584866) and 10
(SRR2589044). The variant records match the previous local run.

Local image ID reported by `docker image inspect ltee-project:0.1.0`:
`sha256:c9491f86f5f5b96ac5c8bc86dd9175544398ce0e40198e343f01af2f62f1b990`.

The GitHub container check builds the image, downloads the data and checks
these counts from a clean clone. A manual run of the Reproduce workflow
also pushes the checked image to `ghcr.io/mingzhi-c/ltee-project:0.1.0`.

Published image from the [checked GitHub run](https://github.com/mingzhi-c/ltee-project/actions/runs/43814749408):

```bash
docker pull --platform linux/amd64 ghcr.io/mingzhi-c/ltee-project@sha256:879b6592b384420b8e706e618d55ed3c391cdefc6b7a77babf4d7e8d2a4bf9ad
```

Use this full image reference in place of `ltee-project:0.1.0` in the run
commands above. The digest identifies the published build even if the
`0.1.0` tag is updated later.

Save an image archive for the release, or load an existing archive:

```bash
docker save ltee-project:0.1.0 | gzip > containers/ltee-project_0.1.0.tar.gz
docker load -i containers/ltee-project_0.1.0.tar.gz
```

The archive is excluded from Git and can be deposited with the release on
Zenodo. Keep the downloaded data and generated results outside Git as well.
