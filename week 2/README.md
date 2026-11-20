# Model Comparison: U-Net vs PVT Attention U-Net

## 1. Overview

Two deep learning architectures were developed and evaluated for image segmentation using the CAMO dataset:

1. **Original U-Net:** A baseline encoder-decoder segmentation architecture.
2. **PVT Attention U-Net:** An enhanced architecture using a pretrained Pyramid Vision Transformer (PVTv2-B0) backbone, skip connections, and channel and spatial attention modules.

The objective was to investigate whether integrating transformer-based visual features and attention mechanisms could improve segmentation performance.

## 2. Dataset and Experimental Setup

Both models were trained and evaluated using the same CAMO dataset split.

| Setting | Value |
|---|---|
| Dataset | CAMO |
| Training images | 1,000 |
| Test images | 250 |
| Input image resolution | 256 × 256 |
| Training epochs | 5 |
| Loss function | BCEWithLogitsLoss |
| Evaluation metrics | S-measure, E-measure, MAE |

The PVT model used ImageNet normalization to match its pretrained backbone requirements. The original U-Net used its corresponding training preprocessing.

## 3. Training Results

Training loss decreased across all five epochs for both architectures.

### Original U-Net

| Epoch | Training Loss |
|---|---:|
| 1 | 0.4835 |
| 2 | 0.4545 |
| 3 | 0.4268 |
| 4 | 0.3652 |
| 5 | 0.3511 |

### PVT Attention U-Net

| Epoch | Training Loss |
|---|---:|
| 1 | 0.2839 |
| 2 | 0.2061 |
| 3 | 0.1917 |
| 4 | 0.1723 |
| 5 | 0.1554 |

Both models showed decreasing training loss, indicating that they learned from the training data.

## 4. Evaluation Metrics

The models were evaluated on the same 250 test images using three metrics.

- **S-measure:** Evaluates structural similarity between predicted segmentation masks and ground-truth masks. Higher values are better.
- **E-measure:** Evaluates alignment between predicted and ground-truth regions. Higher values are better.
- **MAE:** Measures the average absolute pixel-level difference between prediction probabilities and ground-truth masks. Lower values are better.

## 5. Performance Comparison

| Evaluation Metric | Original U-Net | PVT Attention U-Net |
|---|---:|---:|
| S-measure ↑ | 0.5803 | **0.7099** |
| Adaptive E-measure ↑ | 0.6980 | **0.7930** |
| MAE ↓ | 0.2307 | **0.1335** |

### Relative Improvement

The PVT Attention U-Net achieved the following improvements over the original U-Net:

| Metric | Improvement |
|---|---:|
| S-measure | 22.3% |
| Adaptive E-measure | 13.6% |
| MAE reduction | 42.1% |

The relative MAE reduction was calculated using:

`(U-Net MAE - PVT MAE) / U-Net MAE × 100`

## 6. Architecture Decisions

| Component | Decision | Reason |
|---|---|---|
| Backbone | PVTv2-B0 | Uses pretrained visual features |
| Feature extraction | Multi-scale PVT features | Captures information at different resolutions |
| Channel attention | Integrated into decoder | Helps emphasize important feature channels |
| Spatial attention | Integrated into decoder | Helps emphasize relevant spatial regions |
| Skip connections | Included | Preserve spatial information during decoding |
| Backbone training | Frozen | Reduces the number of trainable parameters and computational requirements |

The PVT Attention U-Net is a custom SINet-V2-inspired architecture, not an exact reproduction of SINet-V2.

## 7. Final Decision

Based on the experimental results, PVT Attention U-Net was selected as the better-performing architecture.

It achieved higher S-measure and adaptive E-measure scores and a lower MAE than the original U-Net.

The results suggest that combining pretrained PVT features with channel and spatial attention can improve segmentation performance on the CAMO test dataset.

## 8. Limitations and Future Work

- The models were trained for only five epochs.
- An ablation study is still required to isolate the contribution of the PVT backbone and attention modules.
- Further experiments are needed to evaluate generalization and robustness.
- CAMO is a camouflaged-object detection dataset, not a solar-panel defect dataset. Therefore, these results do not establish performance on actual solar-panel defects.
- Evaluation on an appropriate solar-panel defect dataset is necessary before making application-specific claims.

## 9. Conclusion

The experimental comparison showed that PVT Attention U-Net outperformed the original U-Net across all three evaluation metrics on the CAMO test dataset.

The results support continuing development with the PVT Attention U-Net architecture, followed by ablation experiments and evaluation on domain-specific solar-panel defect data.