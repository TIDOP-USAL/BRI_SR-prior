# BRI-SR prior

**Super-Resolution prior for Multi-Task Building and Road Segmentation from Sentinel-1/2 Imagery**

BRI-SR studies auxiliary image representations for joint building and road segmentation from Sentinel-1 and Sentinel-2 imagery. The framework predicts building and road masks on a 2.5 m grid from observations stored on a 10 m grid.

## Data and trained models

- **Dataset and models — [Google Drive](https://drive.google.com/drive/folders/11OAeqeDaZ2qXe-KszQuE3kC-BZiO3XEE?usp=sharing)**

The dataset contains **39,135 patches across Spain**, including six Sentinel-2 bands, Sentinel-1 VV/VH observations, building and road reference masks, metadata and fixed train/validation/test partitions (**31,294 / 3,924 / 3,917** patches). The download will contain `dataset.zarr.zip`, `metadata.csv`, `splits.csv`, and the dataset summary and documentation. The manuscript configurations use descending-orbit Sentinel-1.

The selected trained models will be distributed under these descriptive names:

- **BRI-SR Bicubic** — Sentinel-1/2 with a bicubic RGB auxiliary image and shared prediction heads.
- **BRI-SR Real-ESRGAN** — Sentinel-1/2 with a frozen Real-ESRGAN RGB prior and shared prediction heads.
- **BRI-SR SEN2SRLite** — Sentinel-1/2 with a frozen SEN2SRLite RGBN prior and shared prediction heads.

## Main results

Validation results reported in the manuscript. Higher is better for all metrics below. IoU-B and IoU-R refer to buildings and roads; BF1-B measures building boundary agreement and clDice-R measures road skeleton coverage.

| Model | mIoU | IoU-B | IoU-R | BF1-B | clDice-R |
| --- | ---: | ---: | ---: | ---: | ---: |
| Sentinel-1/2 baseline, no auxiliary image | 0.4851 | 0.6066 | 0.3637 | 0.4786 | 0.5533 |
| BRI-SR Bicubic | 0.5258 | 0.6363 | 0.4152 | 0.5450 | 0.6018 |
| BRI-SR Real-ESRGAN | 0.5226 | 0.6337 | 0.4115 | 0.5385 | 0.6002 |
| BRI-SR SEN2SRLite | 0.5215 | 0.6329 | 0.4101 | 0.5235 | 0.5991 |

These are validation measurements with building/road thresholds of 0.50/0.20, not held-out test results. The baseline and final configurations differ in architecture and training settings. The final trio also includes training-setting differences, so the comparison describes complete configurations rather than an isolated effect of the prior. The selected SEN2SRLite checkpoint comes from an interrupted training run.

## Experiments

The manuscript presents **15 configurations**, organized into three groups:

1. **Sensor inputs without an auxiliary image (4):** Sentinel-1 only; Sentinel-2 RGB; Sentinel-2 all six bands; and Sentinel-1 + Sentinel-2.
2. **Auxiliary representations under the original configuration (8):** bicubic and Real-ESRGAN with Sentinel-2 RGB, Sentinel-2 all bands, and Sentinel-1/2; SEN2SRLite RGBN with Sentinel-2 all bands and Sentinel-1/2.
3. **Final configurations with shared heads (3):** BRI-SR Bicubic, BRI-SR Real-ESRGAN, and BRI-SR SEN2SRLite, all with Sentinel-1/2 inputs.

The partitions are defined at patch level rather than by geographically held-out regions. Reference masks reflect the coverage and geometry of the source cartography.

This initial release provides an overview and access to the review materials. Reproduction code and detailed experiment documentation will follow in a later release.
