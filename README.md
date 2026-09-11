# Data-driven Personalized Neural Adaptation for Robust Auditory Attention Decoding

## Table of Contents

- [Introduction](#introduction)
- [Personalized Adaptation Layer](#personalized-adaptation-layer)
- [Experimental Scope](#experimental-scope)
- [Additional Material](#additional-material)
- [Public Datasets](#public-datasets)
- [Acknowledgements](#acknowledgements)

## Introduction

This repository accompanies the manuscript **Data-driven Personalized Neural Adaptation for Robust Auditory Attention Decoding**.

Auditory attention decoding (AAD) aims to identify the speaker to whom a listener is attending from electroencephalography (EEG). Its practical use is challenged by inter-subject spatial variability and by the limited availability of subject-specific anatomical information, such as individual MRI, precise electrode digitization, and lead-field matrices.

To address this problem, we propose a **Personalized Adaptation Layer (PAL)**, a lightweight and implementation-compatible front-end for AAD models. PAL is designed to provide subject-specific spatial adaptation while requiring minimal changes to the downstream decoding backbone.

The current repository release provides the additional material associated with the manuscript.

## Personalized Adaptation Layer

PAL consists of two main stages:

1. **Standardized reference anchoring** uses a standard-head-model reference electrode standardization technique (REST) to place EEG signals in a common reference space.
2. **Personalized matrix modulation** performs task-driven, subject-specific channel adaptation through low-rank residual channel mixing, channel-wise scaling, and bias adjustment.

PAL is inserted before the AAD backbone:

```text
EEG input
    -> standard-head-model REST anchoring
    -> personalized low-rank channel modulation
    -> AAD backbone
    -> attention prediction
```

The method does not attempt to reconstruct an individual's anatomy or physical lead field. Instead, it learns task-related channel reweighting in a REST-anchored coordinate system. In the evaluated configurations, PAL adds only 640--8,320 trainable parameters and is jointly optimized with the corresponding backbone.

## Experimental Scope

PAL was evaluated in a within-subject setting using:

- Two public AAD datasets: **KUL** and **DTU**
- Four heterogeneous AAD backbones: **STAnet**, **SSF-CNN**, **DBPNet**, and **DARNet**
- Three decision windows: **0.1 s**, **1 s**, and **2 s**

The primary comparison is the paired change between each backbone and its PAL-equipped counterpart under the same evaluation protocol. Because the native pipelines of the four backbones differ, absolute cross-backbone accuracy should not be interpreted as a causal model ranking.

Across the 24 backbone--dataset--window comparisons reported in the manuscript, all mean changes were positive. The size of the improvement was architecture dependent, supporting PAL as a lightweight, validation-driven front-end rather than a universally beneficial or calibration-free solution.

## Additional Material

The additional document provides additional analyses and complete supporting results, including:

- Published/reference versus reproduced baseline comparisons
- PAL parameter counts, FLOPs, latency, and training-time overhead
- Complete subject-level paired statistical tests with multiplicity correction
- Dependence-robust mixed-effects and cluster-robust analyses
- Native-protocol versus unified-protocol sensitivity analyses
- Train, validation, and test performance comparisons
- Subject-level performance variability analyses
- Hyperparameter sensitivity results

## Public Datasets

The experiments use the following publicly available datasets. Please refer to their official releases for access conditions and dataset documentation:

- **KUL Auditory Attention Dataset:** [Zenodo record 4004271](https://zenodo.org/records/4004271)
- **DTU Auditory Attention Dataset:** [Zenodo record 1199011](https://zenodo.org/records/1199011)

The datasets are not redistributed in this repository.

## Acknowledgements

We thank the researchers who made the KUL and DTU auditory-attention datasets publicly available. We also acknowledge the authors of STAnet, SSF-CNN, DBPNet, and DARNet, which were used as the AAD backbones in our evaluation.

