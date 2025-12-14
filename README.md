# COMP0173 Coursework 2  
## Attention U-Net for Urban Green Space Loss Detection (UK Case Study)

This repository contains the implementation and experimental code for **COMP0173 Coursework 2 – AI for Sustainable Development** at UCL.

The project replicates an Attention U-Net–based remote sensing segmentation framework and adapts it to a **UK urban context**, focusing on woodland and green space loss detection in Greater London using Sentinel-2 satellite imagery.

---

## Overview

The original study **An attention-based U-Net for detecting deforestation within satellite
sensor imagery** by John and Zhang (2022) applies an Attention U-Net architecture to large-scale forest segmentation tasks using Sentinel-2 imagery.  
In this coursework, the methodology is transferred to a new geographic and semantic context characterised by:

- Urban–rural mixed land cover  
- Fragmented and small target regions  
- Severe class imbalance  
- Weak (proxy) supervision derived from NDVI change  

The primary objective is to **evaluate the transferability of the model architecture and training pipeline**, rather than to maximise absolute segmentation performance.

---

## Environment Setup

This project is implemented in **Python 3** using TensorFlow/Keras.

### Dependencies

Key dependencies include:
- Python ≥ 3.8  
- TensorFlow ≥ 2.x  
- NumPy  
- OpenCV  
- Rasterio / GDAL (for geospatial preprocessing)  
- Google Earth Engine Python API (for data acquisition)

A full list of dependencies is documented in the notebooks.

---

## Data Preparation

### Sentinel-2 Imagery
- Source: Sentinel-2 MSI Level-2A (surface reflectance)
- Bands used:  
  - B2 (Blue)  
  - B3 (Green)  
  - B4 (Red)  
  - B8 (NIR)  
- Spatial resolution: 10 m

### Study Area
- Area of Interest: Greater London  
- Time windows:
  - T1: Summer 2018  
  - T2: Summer 2023  

### Proxy Labels
Ground-truth labels are generated using **NDVI difference thresholding** between T1 and T2 to identify potential vegetation loss.  
These labels are treated as **weak (proxy) supervision**, rather than manually annotated ground truth.

---

## Model Architecture

- Architecture: **Attention U-Net**
- Input channels: RGB + NIR (4 channels)
- Base filter size: 16
- Attention gates applied on all skip connections
- Output: single-channel sigmoid segmentation map

The model is trained **from scratch** to isolate architectural transferability without reliance on pretrained backbones.

---

## Training and Evaluation

### Training Configuration
- Optimiser: Adam  
- Loss functions evaluated:
  - Binary Cross-Entropy + Dice loss
  - Focal loss (γ = 2.0, α = 0.25)
- Learning rates: 1e-4, 5e-4
- Early stopping and model checkpointing enabled

### Evaluation Metrics
- Intersection over Union (IoU)
- F1-score (Dice coefficient)
- Precision and Recall

Pixel accuracy is reported for reference only due to severe class imbalance.

---

## Reproducibility

To support reproducibility:
- Fixed random seeds are used
- Deterministic train/validation/test splits are applied
- Hyperparameters, metrics, and checkpoints are logged

---

## Notes on Pretraining

This project intentionally avoids pretrained backbones to focus on **architectural transferability under limited data and weak supervision**.  
While pretraining could improve absolute performance, it would confound evaluation of architecture-level transfer across domains.

---

## Disclaimer

This repository is provided for **academic assessment purposes only**.  
Results should be interpreted as a methodological case study rather than a production-ready monitoring system.
