---

title: "Benchmarking ASR Models for Indian Languages"
weight: 3
date: 2026-08-22
draft: false
description: "A practical research methodology for benchmarking Automatic Speech Recognition models across Indian languages using reproducible datasets, metrics, experiments, and error analysis."
tags:
- Speech Recognition
- ASR
- Indian Languages
- Benchmarking
- Whisper
- SenseVoice
- Machine Learning
- Research
categories:
- Research
- Speech AI

---


## Introduction

Automatic Speech Recognition (ASR) has advanced rapidly with the development of large multilingual speech models.

Models such as **Whisper** and **SenseVoice** have made it possible to experiment with multilingual speech recognition without building an ASR system entirely from scratch.

However, simply running two models on a few audio files and comparing their outputs is not enough to establish a meaningful benchmark.

A reliable benchmark requires a carefully designed experimental methodology.

The central research question is:

> **How can we fairly and reproducibly evaluate modern ASR models across Indian languages?**

This article presents a framework for designing such an experiment.

---

# 1. What Is ASR Benchmarking?

ASR benchmarking is the systematic evaluation of speech recognition systems under controlled conditions.

A basic benchmark looks like:

```text
Speech Dataset
      ↓
   Model A
      ↓
 Transcription
      ↓
   Evaluation
      ↓
 WER / CER

Speech Dataset
      ↓
   Model B
      ↓
 Transcription
      ↓
   Evaluation
      ↓
 WER / CER
```

The results can then be compared.

However, a good benchmark must control the variables that could otherwise influence the result.

---

# 2. The Benchmarking Objective

Before starting an experiment, the research objective should be clearly defined.

A weak objective would be:

> Compare Whisper and SenseVoice.

A stronger objective would be:

> Evaluate the transcription accuracy, robustness, computational requirements, and error patterns of selected multilingual ASR models across representative Indian languages.

The second formulation creates a much broader and more meaningful research framework.

It allows us to investigate:

* accuracy
* language variation
* speaker variation
* noise robustness
* computational cost
* model behavior
* error patterns

---

# 3. Define the Research Questions

A benchmark should begin with research questions.

Possible questions include:

### RQ1

> How accurately do modern multilingual ASR models recognize selected Indian languages?

### RQ2

> How does ASR performance vary between languages?

### RQ3

> How does performance change under noisy recording conditions?

### RQ4

> How does code-switching affect transcription accuracy?

### RQ5

> What types of errors are most common for each model?

### RQ6

> What is the relationship between model size and recognition performance?

### RQ7

> What is the trade-off between ASR accuracy and computational cost?

These questions can guide the entire experimental design.

---

# 4. Selecting the Languages

The benchmark should not simply select languages because they are popular.

Language selection should be based on research criteria.

Possible criteria include:

* dataset availability
* speaker population
* linguistic diversity
* script diversity
* resource availability
* regional representation
* availability of reliable reference transcriptions

A possible benchmark could include:

| Language  | Script     | Example Research Interest |
| --------- | ---------- | ------------------------- |
| Hindi     | Devanagari | High-resource baseline    |
| Marathi   | Devanagari | Regional language         |
| Bengali   | Bengali    | Different script          |
| Tamil     | Tamil      | Dravidian language        |
| Telugu    | Telugu     | Dravidian language        |
| Kannada   | Kannada    | Dravidian language        |
| Gujarati  | Gujarati   | Regional language         |
| Malayalam | Malayalam  | Dravidian language        |
| Punjabi   | Gurmukhi   | Different script          |
| Odia      | Odia       | Regional representation   |

The final language list should depend on the availability and quality of suitable datasets.

---

# 5. Dataset Selection

Dataset selection is one of the most important parts of the benchmark.

A dataset should ideally provide:

* audio files
* reliable transcriptions
* language labels
* speaker information
* sufficient sample size
* licensing suitable for research
* consistent metadata

The dataset should also represent realistic speech.

For example:

```text
Dataset
   ↓
Clean Speech
   +
Different Speakers
   +
Different Regions
   +
Different Recording Devices
   +
Different Speaking Styles
```

A benchmark based on only a few speakers or highly controlled recordings may not generalize to real-world speech.

---

# 6. Reference Transcriptions

ASR evaluation requires a **reference transcription**.

The reference transcription is the text considered to represent what was actually spoken.

For example:

```text
Audio:
[recording.wav]

Reference:
"मला आज पुण्याला जायचे आहे."
```

The ASR model produces:

```text
Prediction:
"मला आज पुण्याला जायचं आहे."
```

The evaluation system compares the prediction against the reference.

The quality of the reference transcription is therefore extremely important.

If the reference itself contains errors, the calculated metrics may be misleading.

---

# 7. Train, Validation, and Test Sets

If model training or fine-tuning is part of the research, the dataset should be divided appropriately.

A common structure is:

```text
Dataset
   |
   ├── Training Set
   |
   ├── Validation Set
   |
   └── Test Set
```

### Training Set

Used to train or fine-tune the model.

### Validation Set

Used to tune parameters and make development decisions.

### Test Set

Used only for final evaluation.

The test set should ideally remain unseen during model development.

---

# 8. Avoiding Data Leakage

Data leakage can produce artificially good results.

For example, if recordings from the same speaker appear in both training and test sets, the model may benefit from having already encountered that speaker's characteristics.

A stronger experimental design uses **speaker-independent splitting**.

```text
Speaker A ──→ Training

Speaker B ──→ Training

Speaker C ──→ Validation

Speaker D ──→ Test
```

This allows us to evaluate how well the model generalizes to unseen speakers.

---

# 9. Candidate ASR Models

A benchmark can compare several models.

For example:

### Whisper

A multilingual speech recognition model that provides multiple model sizes.

### SenseVoice

A multilingual speech foundation model supporting speech recognition and additional speech understanding capabilities.

Other models can be added depending on the research objective and available infrastructure.

The important principle is:

> **The models should be evaluated using the same underlying test data and equivalent evaluation procedures.**

---

# 10. Model Configuration

Model configuration must be documented.

For each model, record:

* model name
* model version
* model size
* framework
* precision
* decoding configuration
* language setting
* task setting
* hardware
* software versions

For example:

```text
Model:
Whisper

Model Size:
Medium

Framework:
PyTorch

Hardware:
NVIDIA GPU

Precision:
FP16

Task:
Transcription

Language:
Marathi
```

The exact configuration should be recorded in the experiment log.

---

# 11. Audio Preprocessing

Different datasets may use different audio formats.

Before inference, audio may need to be standardized.

Potential preprocessing steps include:

```text
Original Audio
      ↓
Format Validation
      ↓
Sample Rate Conversion
      ↓
Channel Conversion
      ↓
Normalization
      ↓
Model Input
```

Possible parameters include:

* sample rate
* mono/stereo
* bit depth
* audio format
* maximum duration

However, preprocessing should be carefully controlled.

Over-processing the audio can unintentionally favor one model.

---

# 12. Establishing a Baseline

Before performing complicated experiments, establish a baseline.

For example:

```text
Dataset
   ↓
Whisper Baseline
   ↓
WER
   ↓
CER
```

Then:

```text
Dataset
   ↓
SenseVoice Baseline
   ↓
WER
   ↓
CER
```

Only after establishing reliable baselines should additional experiments be introduced.

---

# 13. Word Error Rate

**Word Error Rate (WER)** is one of the most common ASR evaluation metrics.

The standard formula is:

```text
WER = (S + D + I) / N
```

Where:

* **S** = substitutions
* **D** = deletions
* **I** = insertions
* **N** = number of words in the reference

Lower WER generally indicates better word-level transcription performance.

Example:

```text
Reference:
I am going to Pune

Prediction:
I am going Pune
```

The word **"to"** has been deleted.

That contributes to the WER.

---

# 14. Character Error Rate

**Character Error Rate (CER)** evaluates errors at the character level.

The formula is similar:

```text
CER = (S + D + I) / N
```

The difference is that characters rather than words are evaluated.

CER can be particularly useful when evaluating languages and writing systems where word segmentation can complicate direct word-level comparison.

A strong benchmark can report both:

```text
Model
  ↓
WER
+
CER
```

---

# 15. Text Normalization

Before calculating WER and CER, text normalization should be defined.

Potential normalization operations include:

* Unicode normalization
* whitespace normalization
* punctuation handling
* number normalization
* capitalization handling where applicable
* special-character handling

For Indian-language ASR, normalization becomes particularly important because different textual representations can affect evaluation.

The normalization rules should be applied consistently to both:

```text
Reference Text
       +
Predicted Text
       ↓
Normalization
       ↓
Evaluation
```

---

# 16. Benchmarking Under Noise

A realistic benchmark should consider more than clean speech.

A possible experiment could introduce controlled noise levels.

For example:

```text
Clean Audio
     ↓
Low Noise
     ↓
Medium Noise
     ↓
High Noise
```

The same model can then be evaluated at each level.

Example:

| Condition    | WER |
| ------------ | --: |
| Clean        |   — |
| Low Noise    |   — |
| Medium Noise |   — |
| High Noise   |   — |

This allows us to measure robustness rather than only clean-condition accuracy.

---

# 17. Code-Switching Evaluation

Another important experiment is code-switched speech.

For example:

> "आज माझी office मध्ये meeting आहे."

A benchmark could compare:

```text
Monolingual Speech
        vs
Code-Switched Speech
```

Possible measurements include:

* WER
* CER
* language identification accuracy
* error categories

The goal is to understand whether the model degrades significantly when multiple languages occur within the same utterance.

---

# 18. Accent and Speaker Evaluation

The benchmark can also group results by speaker characteristics.

For example:

```text
Speaker Group A
     ↓
WER

Speaker Group B
     ↓
WER

Speaker Group C
     ↓
WER
```

If metadata is available, analysis can consider:

* region
* accent
* age group
* speaking rate
* recording device

Care must be taken to avoid inappropriate conclusions from small samples.

---

# 19. Measuring Computational Performance

Accuracy is only one part of ASR model evaluation.

A model may have excellent accuracy but require substantial computational resources.

Therefore, record:

### Inference Time

How long does the model take to process an audio sample?

### Real-Time Factor

A useful concept is **Real-Time Factor (RTF)**.

A simplified definition is:

```text
RTF = Processing Time / Audio Duration
```

For example, if:

```text
Audio duration = 60 seconds
Processing time = 30 seconds
```

then:

```text
RTF = 30 / 60 = 0.5
```

An RTF below 1 indicates that the system processes audio faster than real time under that particular setup.

---

# 20. Memory and Hardware

The benchmark should also record resource requirements.

For example:

| Model   | GPU Memory | Processing Time | WER |
| ------- | ---------: | --------------: | --: |
| Model A |          — |               — |   — |
| Model B |          — |               — |   — |

This enables a more practical comparison.

A model that improves WER by a small amount but requires dramatically more hardware may not be the best choice for deployment.

---

# 21. Accuracy vs Efficiency

This creates an important engineering trade-off:

```text
              Accuracy
                 ↑
                 |
                 |
                 |
                 └────────────→ Computational Cost
```

The objective may not be:

> Find the model with the lowest WER.

Instead, it may be:

> Find the model that provides an appropriate balance between accuracy, speed, memory usage, and deployment cost.

This becomes especially important for edge devices and real-time applications.

---

# 22. Error Analysis

Numerical metrics should be supplemented with qualitative analysis.

Suppose:

```text
Whisper WER = 15%
SenseVoice WER = 17%
```

That does not tell us why Whisper performed better.

We should inspect actual errors.

Possible categories include:

* substitutions
* deletions
* insertions
* proper-name errors
* code-switching errors
* pronunciation errors
* dialect-related errors
* noise-related errors
* technical vocabulary errors
* segmentation errors

A useful error-analysis table might look like:

| Error Category  | Model A | Model B |
| --------------- | ------: | ------: |
| Proper Names    |       — |       — |
| Code Switching  |       — |       — |
| Noise           |       — |       — |
| Dialect         |       — |       — |
| Technical Terms |       — |       — |

---

# 23. Per-Language Evaluation

An overall score can hide important differences.

Consider:

| Language | Model A WER | Model B WER |
| -------- | ----------: | ----------: |
| Hindi    |           — |           — |
| Marathi  |           — |           — |
| Tamil    |           — |           — |
| Telugu   |           — |           — |
| Bengali  |           — |           — |

This is much more informative than:

```text
Overall WER:
Model A = 15%
Model B = 18%
```

The per-language results allow researchers to identify languages where a model performs particularly well or poorly.

---

# 24. Macro vs Micro Averaging

When multiple languages are included, researchers need to think carefully about aggregation.

Suppose:

```text
Hindi:
100,000 words

Marathi:
10,000 words

Tamil:
5,000 words
```

A simple overall score may be dominated by Hindi.

A **macro average** can give each language equal importance.

A **micro average** aggregates all samples together.

Both approaches can be useful, but they answer different questions.

Therefore, a benchmark should clearly state which aggregation method is being used.

---

# 25. Statistical Analysis

If a benchmark evaluates many samples, statistical analysis can help determine whether observed differences are meaningful.

Instead of reporting only:

> Model A WER = 16%

the study can investigate:

* mean WER
* median WER
* standard deviation
* confidence intervals
* per-speaker distributions
* statistical significance

This becomes increasingly important as the number of models and languages increases.

---

# 26. Reproducibility

A research benchmark should be reproducible.

The project should ideally maintain:

```text
ASR-Benchmark/
│
├── data/
├── notebooks/
├── src/
├── configs/
├── results/
├── reports/
├── requirements.txt
└── README.md
```

Configuration files can define:

```text
model
language
dataset
audio settings
decoding parameters
evaluation metrics
```

This makes it possible to rerun experiments consistently.

---

# 27. Experiment Tracking

Each experiment should receive a unique identifier.

For example:

```text
EXP-001
Whisper
Hindi
Clean Audio

EXP-002
Whisper
Marathi
Clean Audio

EXP-003
SenseVoice
Hindi
Clean Audio

EXP-004
SenseVoice
Marathi
Clean Audio
```

A results file could contain:

| Experiment | Model      | Language | Condition | WER | CER | RTF |
| ---------- | ---------- | -------- | --------- | --: | --: | --: |
| EXP-001    | Whisper    | Hindi    | Clean     |   — |   — |   — |
| EXP-002    | Whisper    | Marathi  | Clean     |   — |   — |   — |
| EXP-003    | SenseVoice | Hindi    | Clean     |   — |   — |   — |
| EXP-004    | SenseVoice | Marathi  | Clean     |   — |   — |   — |

This becomes extremely useful when experiments grow.

---

# 28. Example Research Pipeline

A complete benchmarking workflow could be:

```text
                 Research Question
                         ↓
                  Literature Review
                         ↓
                  Dataset Selection
                         ↓
                 Dataset Validation
                         ↓
                 Speaker Splitting
                         ↓
                  Audio Preprocessing
                         ↓
                 ┌───────┴───────┐
                 ↓               ↓
              Whisper        SenseVoice
                 ↓               ↓
             Prediction       Prediction
                 ↓               ↓
                 └───────┬───────┘
                         ↓
                 Text Normalization
                         ↓
                 WER / CER
                         ↓
              Computational Metrics
                         ↓
                  Error Analysis
                         ↓
               Statistical Analysis
                         ↓
                    Results
                         ↓
                  Research Findings
```

---

# 29. What Would Make the Benchmark Research-Grade?

A research-grade benchmark should provide:

### 1. Clear research questions

What are we trying to discover?

### 2. Reliable datasets

Are the audio and reference transcriptions trustworthy?

### 3. Controlled experiments

Are models evaluated under equivalent conditions?

### 4. Reproducibility

Can another researcher reproduce the experiment?

### 5. Multiple metrics

Is the evaluation based on more than one number?

### 6. Error analysis

Do we understand why models fail?

### 7. Statistical analysis

Are differences supported by sufficient evidence?

### 8. Transparent reporting

Are limitations clearly documented?

---

# 30. Common Benchmarking Mistakes

Several mistakes can make an ASR comparison unreliable.

## Mistake 1: Using different datasets

Comparing Model A on Dataset X with Model B on Dataset Y is not a fair direct comparison.

## Mistake 2: Ignoring speaker overlap

The same speakers appearing in training and testing can cause data leakage.

## Mistake 3: Reporting only overall WER

An overall score can hide language-specific problems.

## Mistake 4: Ignoring normalization

Different punctuation or Unicode representations can distort evaluation.

## Mistake 5: Testing too few samples

A benchmark based on a handful of recordings is unlikely to be reliable.

## Mistake 6: Ignoring computational cost

Accuracy without resource requirements is incomplete for practical deployment.

## Mistake 7: Treating correlation as causation

If one language performs worse, we should not immediately conclude that the language itself caused the difference.

Dataset size, recording conditions, speaker distribution, and other factors may contribute.

---

# 31. Proposed Benchmark Architecture

A reusable benchmark framework could look like:

```text
                ASR Benchmark System
                        │
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
     Dataset          Models        Configuration
        │               │               │
        └───────────────┼───────────────┘
                        ↓
                    Inference
                        ↓
                 Normalization
                        ↓
                 Evaluation Engine
                        │
             ┌──────────┼──────────┐
             ↓          ↓          ↓
            WER        CER        RTF
             │          │          │
             └──────────┼──────────┘
                        ↓
                  Error Analysis
                        ↓
                 Statistical Tests
                        ↓
                 Results Database
                        ↓
                 Visualization
                        ↓
                    Report
```

This architecture could eventually be implemented as a reusable research tool.

---

# 32. Potential Visualization

The benchmark results can be visualized using:

### Bar Charts

Compare WER between models.

### Heatmaps

Compare models across languages.

### Box Plots

Show WER distribution across speakers.

### Scatter Plots

Compare accuracy against computational cost.

For example:

```text
                Accuracy
                   ↑
                   |
         Model A  ●
                   |
                   |       ● Model B
                   |
                   |                 ● Model C
                   |
                   └────────────────────────→
                         Computational Cost
```

Visualization can make differences much easier to interpret.

---

# 33. A Possible Initial Experiment

A practical first experiment does not need dozens of languages.

A smaller pilot study could begin with:

```text
Languages:
Hindi
Marathi

Models:
Whisper
SenseVoice

Conditions:
Clean Speech

Metrics:
WER
CER
Inference Time
```

The experiment could then expand:

```text
Phase 1
Hindi + Marathi
        ↓
Phase 2
Additional Indian Languages
        ↓
Phase 3
Noise Robustness
        ↓
Phase 4
Code-Switching
        ↓
Phase 5
Accent / Regional Variation
        ↓
Phase 6
Fine-Tuning
```

This incremental approach makes the research manageable.

---

# 34. From Benchmark to Research Contribution

The benchmark itself may not be the final contribution.

The benchmark should help reveal patterns.

For example:

```text
Benchmark
    ↓
Unexpected Result
    ↓
Investigate
    ↓
Error Analysis
    ↓
Identify Pattern
    ↓
Hypothesis
    ↓
New Experiment
    ↓
Potential Contribution
```

Suppose a model performs well on Hindi but significantly worse on Marathi.

The next question is not simply:

> "Why is Marathi worse?"

Instead, researchers might investigate:

* training data differences
* speaker distributions
* domain differences
* accent variation
* code-switching
* vocabulary
* transcription conventions
* model language coverage

This is where benchmarking becomes a research tool.

---

# 35. Potential Research Extensions

Once the baseline benchmark is complete, several extensions become possible.

### Fine-Tuning

Investigate whether language-specific fine-tuning improves performance.

### Parameter-Efficient Fine-Tuning

Explore techniques that adapt models without retraining all parameters.

### Data Augmentation

Generate additional training examples using:

* noise augmentation
* speed perturbation
* pitch modification
* synthetic speech

### Domain Adaptation

Evaluate speech from specific domains such as:

* healthcare
* education
* meetings
* customer service
* technical conversations

### Low-Resource Adaptation

Study methods for improving languages with limited labeled speech data.

---

# 36. From Indian ASR to Multilingual Foundation Models

A longer-term research direction is to understand how multilingual speech foundation models represent Indian languages.

A conceptual research pipeline could be:

```text
Indian Speech
      ↓
Multilingual Foundation Model
      ↓
Language Representation
      ↓
Recognition
      ↓
Language Understanding
      ↓
Translation / Generation
```

This connects ASR with the broader field of multilingual AI.

It also creates opportunities to investigate the intersection of:

* speech
* NLP
* foundation models
* generative AI
* multilingual learning
* low-resource learning

---

# 37. Limitations of This Benchmarking Framework

A proposed benchmark methodology also has limitations.

### Dataset Availability

Not every Indian language has an equally large or diverse public dataset.

### Annotation Quality

Reference transcriptions may contain errors.

### Model Updates

Foundation models can change over time.

### Hardware Differences

Inference speed can vary significantly across hardware.

### Metric Limitations

WER and CER do not perfectly represent semantic correctness.

### Language Complexity

Languages cannot always be compared fairly using exactly the same assumptions.

### Sample Size

Small datasets may produce unstable estimates.

These limitations should be explicitly documented in any final research publication.

---

# 38. Proposed Research Roadmap

A complete research project could follow this roadmap:

```text
Phase 1
Literature Review
        ↓
Phase 2
Research Questions
        ↓
Phase 3
Language Selection
        ↓
Phase 4
Dataset Selection
        ↓
Phase 5
Benchmark Infrastructure
        ↓
Phase 6
Baseline Evaluation
        ↓
Phase 7
Multilingual Comparison
        ↓
Phase 8
Noise / Accent / Code-Switching
        ↓
Phase 9
Error Analysis
        ↓
Phase 10
Statistical Evaluation
        ↓
Phase 11
Research Gap Identification
        ↓
Phase 12
Proposed Improvement
        ↓
Phase 13
Validation
        ↓
Research Contribution
```

---

# 39. Conclusion

Benchmarking ASR models across Indian languages is more than comparing two model scores.

A meaningful benchmark must consider:

```text
Language
+
Dataset
+
Speakers
+
Recording Conditions
+
Model
+
Preprocessing
+
Evaluation
+
Computational Cost
+
Error Patterns
```

The goal is not simply to determine:

> **Which ASR model is the best?**

The more interesting research question is:

> **Under what conditions does each model perform well or poorly, and why?**

That question opens the door to deeper research into multilingual speech recognition, low-resource languages, code-switching, robustness, model adaptation, and speech foundation models.

For **The Compute Lab**, this benchmarking framework can become the foundation for an experimental research series.

The next logical step is to move from methodology to a concrete model comparison:

> **Whisper vs SenseVoice for Indian Languages**

That article can introduce both models, explain their architectures at a high level, define a fair comparison methodology, and then eventually lead into actual experimental results.

---

## Research Note

This article presents a proposed benchmarking methodology rather than claiming experimental results.

Actual performance values should be generated through controlled experiments using clearly documented datasets, model versions, configurations, and evaluation procedures.

The objective is to build a **reproducible research framework**, not to assume the outcome before conducting the experiment.
