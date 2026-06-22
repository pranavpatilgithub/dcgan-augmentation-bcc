# Self-Attention DCGAN for Synthetic Augmentation and Basal Cell Carcinoma Classification

This repository contains the PyTorch implementation of the **SA-SN-DCGAN (Self-Attention Spectral Normalization DCGAN)**, a proposed architecture for synthesizing high-fidelity Basal Cell Carcinoma (BCC) clinical images to overcome limited training data in dermatological datasets. 

This work supports the methodology and findings discussed in the associated research paper: *Self-Attention DCGAN for Synthetic Augmentation and Basal Cell Carcinoma Classification*.

## Overview

Generating realistic medical images is challenging, particularly for skin cancer lesions like BCC that contain complex, localized visual features. This repository addresses these challenges by enhancing a standard DCGAN with:
- **Self-Attention Modules:** Introduced at the 48x48 resolution layer to help the generator capture long-range dependencies and intricate localized features of BCC lesions.
- **Spectral Normalization:** Applied to the discriminator layers to enforce Lipschitz continuity, stabilizing the adversarial training process and preventing mode collapse.

The repository includes both the baseline DCGAN and the proposed SA-SN-DCGAN to allow for a direct comparative analysis.

## Features

- **Standard DCGAN Baseline:** A standard Deep Convolutional GAN architecture used for comparative evaluation.
- **Proposed SA-SN-DCGAN:** An improved GAN architecture utilizing `SelfAttention` and `spectral_norm` for enhanced stability and image fidelity.
- **Quantitative Evaluation:** Built-in calculation of industry-standard metrics:
  - **FID (Fréchet Inception Distance):** To measure the realism and diversity of generated images.
  - **IS (Inception Score):** To measure the quality and distinctiveness of generated images.
  - **SSIM (Structural Similarity Index Measure):** To evaluate the structural integrity compared to real clinical images.
- **Training Stability Analysis:** Automatic plotting of Generator and Discriminator loss curves to compare the stability of the proposed model versus the baseline.
- **Self-Attention Visualization:** A utility to visualize the generator's attention map, demonstrating how the network focuses on specific lesion characteristics during synthesis.
- **Synthetic Augmentation Pipeline:** Functions to automatically generate and save synthetic datasets (e.g., producing a 75% synthetic split) for downstream classification tasks.

## Requirements

Ensure you have the following libraries installed:

```bash
pip install torch torchvision numpy scipy scikit-image matplotlib Pillow
```

## Dataset Configuration

The code is pre-configured to run in a Kaggle environment using a dataset of BCC skin cancer images. By default, the dataset path is set to:

```python
TRAIN_DIR = "/kaggle/input/datasets/pranavpatil00/bcc-skincancer/BCC_skincancer/Train"
```

To run this locally, update the `TRAIN_DIR` variable in the `Config` class of `dcgan.py` to point to your local dataset directory containing BCC images.

## Usage

You can execute the entire pipeline by simply running the `dcgan.py` script:

```bash
python dcgan.py
```

### Script Execution Flow:
1. **Baseline Training:** Trains the standard DCGAN for 400 epochs and generates a batch of synthetic images (`/kaggle/working/synthetic_bcc`).
2. **Proposed Model Training:** Trains the SA-SN-DCGAN for 400 epochs and generates a batch of synthetic images (`/kaggle/working/synthetic_bcc_proposed`).
3. **Evaluation:** Computes and prints a comparison table of FID, IS, and SSIM scores between the baseline and proposed models.
4. **Loss Curve Visualization:** Displays a side-by-side plot of the generator and discriminator losses for both models.
5. **Attention Map Visualization:** Generates and displays an overlay of the generator's self-attention map on a synthesized image.

## Results & Findings

As detailed in the accompanying paper, the integration of Self-Attention and Spectral Normalization significantly improves upon the baseline DCGAN:
- **Improved Metrics:** The proposed model achieves better (lower) FID and higher IS and SSIM scores.
- **Enhanced Stability:** Spectral normalization minimizes extreme loss fluctuations, leading to a much smoother training trajectory.
- **Higher Fidelity:** Visual inspection and attention maps show that the SA-SN-DCGAN captures critical textural and structural details of BCC lesions that the baseline fails to resolve.
