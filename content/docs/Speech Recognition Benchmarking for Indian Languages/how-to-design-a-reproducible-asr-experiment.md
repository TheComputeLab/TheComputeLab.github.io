---
title: "How to Design a Reproducible ASR Experiment"
weight: 13
date: 2026-08-22
draft: false
description: "A practical framework for designing ASR experiments that can be repeated, audited, compared, and extended."
tags:
- Reproducible Research
- ASR
- Speech Recognition
- Indian Languages
- Machine Learning
- Experiment Design
- MLOps
- Research
categories:
- Research
- Speech AI
---

## Introduction

ASR results depend on many variables:

```text
Model
Dataset
Preprocessing
Decoding
Normalization
Hardware
Software
Randomness
```

A reproducible experiment makes these variables explicit.

## 1. Research Question and Hypothesis

Example:

> Does multilingual training improve Marathi ASR compared with a Marathi-only baseline?

Hypothesis:

> Multilingual training will reduce Marathi WER when training is appropriately balanced.

## 2. Define Variables

Independent variables may include model, language, training data, noise, and sampling strategy.

Dependent variables may include WER, CER, RTF, and memory.

## 3. Freeze the Dataset

Record the exact dataset version and preserve the benchmark test set.

## 4. Freeze the Model

Record model identifier, version, checkpoint, and relevant configuration.

## 5. Freeze Configuration

Example:

```yaml
model: model-name
dataset: dataset-v1
language: Marathi
sample_rate: 16000
batch_size: 8
normalization: standard_v1
```

## 6. Version Control

Keep experiment code under Git and record the commit used for each run.

## 7. Environment

Record:

```text
Operating System
Python Version
CUDA Version
Framework Version
Library Versions
GPU
CPU
RAM
```

## 8. Random Seeds

Record seeds where randomness is present.

## 9. Preprocessing

Document resampling, channel conversion, segmentation, silence handling, and normalization.

## 10. Experiment IDs

Use:

```text
EXP-001
EXP-002
EXP-003
```

and store model, dataset, language, condition, metrics, timestamp, and code version.

## 11. Baseline First

Start with a baseline before adding improvements.

```text
Baseline
   ↓
Fine-Tuning
   ↓
Augmentation
   ↓
Sampling Strategy
```

## 12. One Change at a Time

Avoid changing model, dataset, preprocessing, and decoder simultaneously when trying to understand causality.

## 13. Save Raw Outputs

Save:

```text
Audio ID
Reference
Hypothesis
Language
Speaker
Experiment ID
```

Example:

```json
{
  "audio_id": "audio_001",
  "language": "Marathi",
  "reference": "मला पुण्याला जायचे आहे",
  "hypothesis": "मला पुण्याला जायचं आहे",
  "experiment": "EXP-001"
}
```

## 14. Directory Structure

```text
asr-experiment/
├── configs/
├── data/
├── src/
│   ├── preprocess.py
│   ├── inference.py
│   └── evaluate.py
├── outputs/
├── notebooks/
├── reports/
└── README.md
```

## 15. Reproducibility Checklist

```text
[ ] Dataset version recorded
[ ] Model checkpoint recorded
[ ] Code version recorded
[ ] Configuration saved
[ ] Environment recorded
[ ] Seed recorded
[ ] Preprocessing documented
[ ] Normalization documented
[ ] Raw predictions saved
[ ] Metrics saved
[ ] Hardware documented
[ ] Evaluation script preserved
```

## Conclusion

A reproducible ASR experiment is a complete record of the question, hypothesis, data, model, configuration, environment, code, evaluation, and raw results.

The goal is simple:

> **Another researcher should be able to understand how a result was produced and have enough information to repeat the experiment.**

