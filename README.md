<div align="center">

```
██████╗ ███████╗███╗   ██╗ ██████╗ ██╗███████╗███████╗
██╔══██╗██╔════╝████╗  ██║██╔═══██╗██║██╔════╝██╔════╝
██║  ██║█████╗  ██╔██╗ ██║██║   ██║██║███████╗█████╗  
██║  ██║██╔══╝  ██║╚██╗██║██║   ██║██║╚════██║██╔══╝  
██████╔╝███████╗██║ ╚████║╚██████╔╝██║███████║███████╗
╚═════╝ ╚══════╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝╚══════╝╚══════╝
          A U T O E N C O D E R
```

**Convolutional Autoencoder for Image Denoising**  
*X-ray Medical Imaging · Fashion-MNIST · Latent Space Analysis*

[![Python](https://img.shields.io/badge/Python-3.9+-3776ab?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)](https://pytorch.org)
[![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white)](https://opencv.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)

</div>

---

## What Is This?

This project trains **Convolutional Autoencoders** to remove noise from images — learning a compressed latent representation that captures the *signal*, not the static. Trained and evaluated across two domains:

| Domain | Dataset | Use Case | Link
|--------|---------|----------|-------|
| 🩻 Medical | X-ray Images | High-stakes denoising for diagnostics | https://www.kaggle.com/code/paultimothymooney/detecting-pneumonia-in-x-ray-images |
| 👗 General | Fashion-MNIST | Benchmark reconstruction quality | imported |

The pipeline covers everything: noise injection → encoder–decoder training → quantitative evaluation → latent space visualization → classical baseline comparison → U-Net skip connection  

---

## ⚙️ Architecture

```
Noisy Input
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│  ENCODER                                                     │
│  Conv → ReLU → MaxPool → Conv → ReLU → MaxPool → ...        │
│                        ↓                                     │
│               [ Latent Bottleneck ]                          │
│                        ↓                                     │
│  DECODER                                                     │
│  ConvTranspose → ReLU → ConvTranspose → ... → Tanh          │
└─────────────────────────────────────────────────────────────┘
    │
    ▼
Clean Reconstruction
```

All models share the **Encoder → Bottleneck → Decoder** skeleton. The experiments vary depth, channel capacity, and spatial compression to study the reconstruction–denoising tradeoff.

---

## 🔬 Ablation Study

Three architectural variants were evaluated to understand what drives denoising quality:

### Experiment 1 — Strong Compression
> Deep encoder, heavy pooling, small bottleneck


### Experiment 2 — High Capacity *(best overall)*
> Deeper conv blocks, wider bottleneck channels


### Experiment 3 — High Spatial Retention
> Fewer pooling layers, larger spatial bottleneck


---

## 📊 Evaluation Metrics

Performance is measured across three complementary axes:

| Metric | Meaning | Direction |
|--------|---------|-----------|
| **MSE** | Average pixel-level reconstruction error | ↓ Lower is better |
| **SSIM** | Structural similarity — edges, contrast, luminance | ↑ Higher is better |
| **PSNR** | Signal-to-noise ratio in reconstructed images (dB) | ↑ Higher is better |

### Results

| Model | Dataset | MSE ↓ | SSIM ↑ | PSNR ↑ |
|-------|---------|-------|--------|--------|
| Baseline AE | X-ray | 0.0056 | 0.8151 | 22.5925 |
| Exp 1 | X-ray | 0.0070 | 0.7708 | 21.5887 |
| Exp 2 | X-ray | 0.0049 | 0.8361 | 23.2090 |
| Exp 3 | X-ray | 0.0044 | 0.8414 | 23.7836 |

---

## 🧬 Latent Space Analysis

The bottleneck isn't just functional — it's *informative*. We extract and visualize latent representations to understand what the model has learned.

**Pipeline:**
```
Extract bottleneck tensors → Flatten → Reduce (t-SNE) → Plot
```

**Visualizations produced:**
- 🟣 **2D and 3D t-SNE** — non-linear cluster topology

**Insights revealed:**
- Cluster formation patterns per class/domain
- How noise corrupts latent structure

---

## 🖼️ Classical Baseline: OpenCV

To contextualize the learned approach, a handcrafted **OpenCV denoising pipeline** is implemented as a reference point.

```
Autoencoder (learned features) vs. OpenCV filters (handcrafted rules)
```

Result: Deep learning consistently outperforms classical methods on both perceptual and structural metrics — especially at higher noise levels.

---
## 🖼️ U-Net skip connection
implemented a U-Net skip connection for experiment 3 model (best model) to analyse how performance will improve in terms of denoising and reconsrtruction
## 🗂️ Repository Structure

```
project_4/
│
├── Trained_xray_model/        # Saved X-ray autoencoder weights (Baseline)
├── Trained_fashion_model/     # Saved Fashion autoencoder weights (Baseline)
│
├── Exp1/                      # High compression architecture
├── Exp2/                      # High capacity architecture
├── Exp3/                      # High spatial retention architecture
│
├── Evaluation/
│   └── Baseline/              # MSE, SSIM, PSNR results + embeddings
│   └── Exp1/                  # MSE, SSIM, PSNR results + embeddings
│   └── Exp2/                  # MSE, SSIM, PSNR results + embeddings
│   └── Exp3/                  # MSE, SSIM, PSNR results + embeddings
│   └── Overall_Evaluation     # Evaluating best experiment results
│
├── OpenCV/                    # Classical denoising baseline with MSE loss
├── UNetExp3/                  # U-Net skip connection
├── main/                      # Training pipeline & core scripts
└── README.md
```


---


<div align="center">

*Built as part of an experimental deep learning study on image denoising and representation learning.*

</div>
