# urban-climate-downscaling via Physics-Informed Diffusion and Graph Neural Operators

Official implementation of the paper:

**"A Trustworthy Deep Learning Framework for Urban Climate Downscaling 
via Physics-Informed Diffusion and Topology-Aware Graph Neural Operators"**
*Mariam Labib, et al. — Expert Systems With Applications (Under Review), 2026*

---

## Overview

This repository provides the complete implementation of a two-stage 
urban climate downscaling framework:

- **Stage 1 — SPIDM** (Stochastic Physics-Informed Diffusion Model): 
  Corrects systematic biases in coarse ERA5 reanalysis data while 
  quantifying predictive uncertainty via stochastic differential equations.

- **Stage 2 — DeepGFNO** (Deep Graph Fourier Neural Operator): 
  Performs topology-aware spatial downscaling on a hybrid graph encoding 
  both geographic proximity and ecological similarity (NDVI).

The framework is evaluated on ERA5 reanalysis data for Cairo and 
Alexandria, Egypt, achieving:
- 54% reduction in wind field divergence error
- 27% improvement in prediction stability (Lipschitz constant)
- 27% better zero-shot transfer to unseen cities vs. U-Net baseline

---

## Repository Structure

```
├── Results.ipynb              # Main pipeline notebook (full implementation)
├── requirements.txt           # Python dependencies
├── data/
│   └── README_data.md         # Instructions to download ERA5 and Sentinel-2
├── models/
│   └── README_models.md       # Model architecture descriptions
└── README.md
```

---

## Requirements

```bash
pip install torch torchvision
pip install torch-geometric
pip install torchsde
pip install xarray rioxarray rasterio
pip install scipy numpy pandas matplotlib seaborn
pip install tqdm psutil
```

**Python version:** 3.9+  
**PyTorch version:** 1.13+  
**Hardware:** GPU recommended (tested on Google Colab T4)

---

## Data

### ERA5 Reanalysis Data
Download via the Copernicus Climate Data Store:
- Variables: 2m temperature (t2m), 10m U-wind (u10), 10m V-wind (v10)
- Resolution: 0.25° × 0.25°
- Period: January–December 2023
- Regions: Cairo (29.0°N–31.5°N, 30.0°E–32.5°E), 
           Alexandria (29.2°N–33.2°N, 27.9°E–31.9°E)
- Link: https://cds.climate.copernicus.eu

### Sentinel-2 NDVI
Generated via Google Earth Engine (GEE):
- Cloud-free annual median composite for 2023
- Bands B4 (Red) and B8 (NIR) for NDVI computation
- Native resolution: 10m × 10m, aggregated to 17×17 grid
- GEE script instructions provided in `data/README_data.md`
---

## Hyperparameters

All hyperparameters are consolidated in Table A1 of the paper. 
Key values:

| Component | Parameter | Value |
|---|---|---|
| SPIDM | Optimizer | Adam, lr=5×10⁻⁴ |
| SPIDM | Epochs | 75 |
| SPIDM | Physics loss weight λ | tuned on validation |
| DeepGFNO | Hidden features | 64 |
| DeepGFNO | Spectral modes | 128 |
| DeepGFNO | Layers | 4 |
| DeepGFNO | Optimizer | Adam, lr=5×10⁻⁴ |
| DeepGFNO | Epochs | 100 |
| Graph | Balance parameter α | 0.7 |
| Graph | Geographic bandwidth σ_d | 1.5 |
| Graph | Ecological bandwidth σ_n | 0.5 |

---

## Results

| Model | RMSE↓ (K) | Div. Error↓ | Lipschitz↓ | Transfer RMSE↓ (K) |
|---|---|---|---|---|
| U-Net | 1.153 | 1.076×10⁻¹ | 0.323 | 4.116 |
| **DeepGFNO (Ours)** | **2.014** | **4.892×10⁻²** | **0.235*** | **3.011** |

*Full results in Tables 2–6 of the paper.*

---

## Citation

If you use this code in your research, please cite:

```bibtex
@article{labib2025trustworthy,
  title={A Trustworthy Deep Learning Framework for Urban Climate 
         Downscaling via Physics-Informed Diffusion and 
         Topology-Aware Graph Neural Operators},
  author={Labib, Mariam and others},
  journal={Expert Systems With Applications},
  year={2025},
  note={Under Review}
}
```


---

## Contact

**Mariam Labib**  
PhD Candidate, Manosura University
Assistant Lecturer, Elsewedy University of Technology  
Cairo, Egypt  
📧 mariamlabib90@std.mans.edu.eg  
🔗 [Google Scholar]([[https://scholar.google.com/citations?user=ljk1Yv0AAAAJ])
```
