# Vocal Isolation Using Dual-Path Networks

## Overview

This project investigates the use of deep learning for music source separation, with a particular focus on vocal isolation. The work is inspired by the SpeechBrain source separation framework and employs a Dual-Path Network architecture to separate vocals from musical accompaniment.

The project explores how model capacity, training duration, and separation complexity affect the quality of vocal isolation using the MUSDB18 dataset and a custom-prepared subset of full-length music tracks.

## Project Goals

The primary objectives of this project were:

* Design and implement a deep learning pipeline for vocal isolation.
* Adapt and evaluate a Dual-Path source separation architecture inspired by SpeechBrain.
* Investigate the effect of model dimensionality on separation quality.
* Analyze the impact of training duration on performance.
* Compare two-stem separation (vocals, drums) against four-stem separation (vocals, drums, bass, other).

---

## Dataset

### MUSDB18

The project utilizes the MUSDB18 music source separation dataset.

Two versions of the dataset were used:

1. **MUSDB18 Preview Dataset**

   * 7-second excerpts provided through the `musdb` package.
   * Used for rapid prototyping and baseline validation.

2. **Custom MUSDB18 Dataset**

   * Full-length audio tracks exported as individual WAV files.
   * Training Set: 50 songs
   * Test Set: 18 songs
   * Sample Rate: 44.1 kHz

---

## Model Architecture

The separation system is based on a Dual-Path Network architecture consisting of:

### Encoder

* Kernel Size: 16
* Stride: 8

### Transformer Blocks

* Intra-Chunk Transformer
* Inter-Chunk Transformer
* Positional Encoding
* Layer Normalization

### Mask Network

* Dual-Path Model
* Configurable for:

  * 2-source separation
  * 4-source separation

### Decoder

* Kernel Size: 16
* Stride: 8

The architecture was adapted from SpeechBrain's LibriMix source separation recipe while being scaled to fit available computational resources.

---

## Experiments

### Experiment 1: Baseline Vocal Isolation

#### Objective

Establish a functional training pipeline using short MUSDB18 excerpts.

#### Configuration

* Dataset: MUSDB18 Preview
* Sources: Vocals + Drums
* Epochs: 200

#### Results

| Metric             | Value    |
| ------------------ | -------- |
| SI-SNR             | -0.25 dB |
| SI-SNR Improvement | 4.51 dB  |
| SDR                | 0.06 dB  |
| SDR Improvement    | 5.14 dB  |

#### Findings

The experiment successfully validated the training pipeline and demonstrated basic source separation capability. While improvements over the input mixture were observed, the overall separation quality remained limited due to the small dataset size and constrained training conditions.

---

### Experiment 2: Impact of Model Dimensionality

#### Objective

Investigate how model capacity and training duration affect separation performance.

#### Configurations

##### Low-Dimensional Model

* Encoder Output: 32
* Output Channels: 32
* FFN Dimension: 32

##### High-Dimensional Model

* Encoder Output: 64
* Output Channels: 64
* FFN Dimension: 64

#### Results

##### Low-Dimensional Model (200 Epochs)

| Metric | Value    |
| ------ | -------- |
| SDR    | -0.95 dB |
| SIR    | 11.60 dB |
| SAR    | -0.12 dB |

##### High-Dimensional Model (200 Epochs)

| Metric | Value    |
| ------ | -------- |
| SDR    | -1.16 dB |
| SIR    | 11.13 dB |
| SAR    | -0.17 dB |

##### Low-Dimensional Model (500 Epochs)

| Metric | Value    |
| ------ | -------- |
| SDR    | -0.46 dB |
| SIR    | 11.99 dB |
| SAR    | 0.37 dB  |

#### Findings

Longer training consistently improved performance across all evaluation metrics. Increasing model dimensionality alone did not improve results, suggesting that training duration and optimization strategy were more influential than model size under the available computational constraints.

---

### Experiment 3: Two-Stem vs Four-Stem Separation

#### Objective

Evaluate the effect of separation complexity by comparing:

* Two-Stem Separation

  * Vocals
  * Drums

versus

* Four-Stem Separation

  * Vocals
  * Drums
  * Bass
  * Other

#### Configuration

* Encoder Output: 64
* Output Channels: 64
* FFN Dimension: 64
* Epochs: 300

#### Results

| Metric | Value    |
| ------ | -------- |
| SDR    | -0.70 dB |
| SIR    | 0.82 dB  |
| SAR    | 8.45 dB  |

#### Findings

The four-stem model achieved higher SDR and SAR values but lower SIR compared to the two-stem model. This suggests a trade-off between preserving overall source quality and minimizing interference when separating a larger number of sources simultaneously.

---

## Sample Outputs

To keep the notebook lightweight and GitHub-compatible, embedded audio playback outputs were removed.

Example separated stems are available in:

```text
samples/
└── output_samples/
    ├── Drums.wav
    ├── Drums_hat.wav
    ├── Vocals.wav
    └── Vocals_hat.wav
```

These files can be used to qualitatively evaluate the separation performance of the model.

---

## Key Findings

* A Dual-Path architecture can successfully perform vocal isolation on music recordings.
* Dataset size and diversity strongly influence separation quality.
* Longer training durations provide more benefit than simply increasing model dimensionality.
* Separating fewer sources can improve interference reduction for target stems.
* Four-stem separation introduces additional complexity and resource requirements.

---

## Future Work

Potential directions for improvement include:

* Training on the complete MUSDB18 dataset.
* Increasing model depth and Transformer capacity.
* Hyperparameter optimization.
* Data augmentation techniques.
* Multi-GPU training for longer audio segments.
* Integration of modern architectures such as Conv-TasNet, SepFormer, or Hybrid Transformer models.
* Evaluation using perceptual audio quality metrics and listening studies.

---

## Author

**Ahmad Hasan**

COMP 691 – Pure Music Project

Concordia University
