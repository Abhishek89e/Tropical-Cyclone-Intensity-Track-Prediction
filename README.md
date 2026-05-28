# Tropical-Cyclone-Intensity-Track-Prediction
# Tropical Cyclone Intensity & Track Prediction

Deep learning models for predicting tropical cyclone intensity and track from satellite imagery, built as part of an MSc Applied Research Project at Dublin Business School.

## Overview

This project compares two deep learning architectures trained on the TCIR (Tropical Cyclone Image Repository) satellite dataset:

- **CNN** — a single-image convolutional model predicting intensity from one satellite frame
- **ConvLSTM** — a sequence-based model using 4 consecutive frames to capture storm evolution over time

Both models predict maximum sustained wind speed (Vmax, in knots) and storm centre coordinates (lat/lon), with additional environmental features derived from multi-channel satellite imagery.

## Results

| Model | RMSE (knots) | R² |
|---|---|---|
| TCI-Net CNN | ~13.6 | ~0.74 |
| ConvLSTM-CNN | ~17.6 | ~0.61 |

Both models outperform the climatological baseline and produce results consistent with comparable published studies (13–20 knot RMSE range).

## Dataset

**TCIR — Tropical Cyclone Image Repository**

Three HDF5 files covering Atlantic, Eastern/Central/Western Pacific, Indian Ocean, and Southern Hemisphere basins:

- `TCIR-ATLN_EPAC_WPAC.h5` — 47,381 samples
- `TCIR-CPAC_IO_SH.h5` — 23,118 samples
- `TCIR-ALL_2017.h5` — 4,580 samples (2017 test year)

Each sample contains a 201×201×4 satellite image with 4 channels (infrared, passive microwave, water vapour, visible) plus storm metadata (Vmax, MSLP, lat/lon, R35).

The dataset is not included in this repo due to file size. Download it from the [TCIR project page](https://github.com/BoyoChen/TCIR) and place the `.h5.tar.gz` files in your Google Drive under `MyDrive/TCIR/`.

## Features Used

| Category | Feature | Source |
|---|---|---|
| Storm | Max sustained wind (Vmax, knots) | TCIR metadata |
| Storm | Min central pressure (MSLP, hPa) | TCIR metadata |
| Storm | Centre lat/lon (track target) | TCIR metadata |
| Storm | Translation speed & direction | Derived from lat/lon + timestamps |
| Storm | RMW proxy | Derived from IR radial gradient |
| Environmental | SST signal | TCIR channel 0 (IR) |
| Environmental | Mid-level moisture | TCIR channel 2 (WV) |
| Environmental | Wind shear proxy | Derived from multi-channel texture |
| Large-scale | ENSO state | NOAA ERSSTv5 Niño3.4 index |
| Large-scale | MJO phase | NOAA CPC RMM index |

## Setup

This notebook runs on **Google Colab** with a T4 GPU runtime.

1. Upload the TCIR `.h5.tar.gz` files to `MyDrive/TCIR/` in your Google Drive
2. Open `TC_Complete.ipynb` in Google Colab
3. Set the runtime to GPU: `Runtime → Change runtime type → T4 GPU`
4. Run all cells in order

Minimum recommended RAM: 6 GB (use High-RAM runtime if available).

## Requirements

All dependencies are available in the default Colab environment:

```
tensorflow >= 2.19.0
numpy
pandas
h5py
opencv-python
scikit-learn
matplotlib
seaborn
psutil
```

## Repo Structure

```
.
├── TC_Complete.ipynb       # Main notebook — full pipeline for both models
└── README.md
```

## Key References

- Wei et al. (2020) — TCIR dataset
- Shi et al. (2015) — ConvLSTM architecture
- Pradhan et al. (2017) — CNN-based cyclone intensity estimation
- Chen et al. (2019) — Multi-channel deep learning for typhoon intensity

## License

This project was produced for academic research purposes at Dublin Business School. The TCIR dataset is subject to its own terms of use — see the [original dataset repository](https://github.com/BoyoChen/TCIR).
