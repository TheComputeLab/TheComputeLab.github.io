---

title: "Whisper vs SenseVoice for Indian Languages"
weight: 4
date: 2026-08-22
draft: false
description: "A research-oriented comparison of Whisper and SenseVoice, with a proposed methodology for evaluating multilingual speech recognition on Indian languages."
tags:
- Whisper
- SenseVoice
- Speech Recognition
- ASR
- Indian Languages
- Multilingual AI
- Machine Learning
- Research
categories:
- Research
- Speech AI

---

## Introduction

Modern speech recognition has changed significantly with the development of large multilingual speech models.

Two interesting systems for studying modern speech recognition are **Whisper** and **SenseVoice**.

Whisper was developed by OpenAI as a general-purpose speech recognition model trained on a large and diverse multilingual dataset. It supports multilingual speech recognition, speech translation, and language identification.

SenseVoice is a speech foundation model developed within the FunAudioLLM project. It is designed for multiple speech understanding tasks, including automatic speech recognition, spoken language identification, speech emotion recognition, and audio event detection.

This makes the two systems interesting candidates for comparative research.

However, an important research principle is:

> **A model comparison should be based on controlled experiments rather than assumptions about which model is better.**

This article therefore examines the two model families and proposes an experimental framework for studying their behavior on Indian-language speech.

---

# 1. Why Compare Whisper and SenseVoice?

The two systems represent interesting approaches to multilingual speech processing.

Whisper uses an **encoder-decoder Transformer** architecture and was trained using approximately 680,000 hours of multilingual and multitask supervised audio data. Its training supports tasks including multilingual transcription, translation, and language identification.

SenseVoice uses a different approach. The SenseVoice documentation describes it as a **non-autoregressive speech foundation model** with an encoder-based architecture and CTC decoding, designed to provide speech recognition alongside additional speech-understanding capabilities.

This creates an interesting research comparison:

```text
                    Speech
                      ↓
             ┌────────┴────────┐
             ↓                 ↓
          Whisper          SenseVoice
             ↓                 ↓
        Transcription     Transcription
             ↓                 ↓
             └────────┬────────┘
                      ↓
                  Evaluation
                      ↓
               WER / CER / RTF
```

The purpose is not to assume a winner.

The purpose is to understand:

> **How do different speech-model architectures behave under the same experimental conditions?**

---

# 2. Understanding Whisper

Whisper is an automatic speech recognition system released by OpenAI.

The model is designed as a general-purpose multilingual and multitask speech model.

Its training uses approximately:

> **680,000 hours of multilingual and multitask supervised audio data.**

The Whisper research describes 117,000 hours covering 96 languages other than English, along with substantial speech-translation data.

The model uses an encoder-decoder Transformer architecture.

A simplified representation is:

```text
Audio
  ↓
Log-Mel Spectrogram
  ↓
Encoder
  ↓
Decoder
  ↓
Text Tokens
  ↓
Transcription
```

Whisper processes audio in approximately 30-second windows during transcription.

---

# 3. Whisper Model Sizes

Whisper provides multiple model sizes.

The official model card lists:

| Model  | Approx. Parameters | Multilingual |
| ------ | -----------------: | ------------ |
| Tiny   |                39M | Yes          |
| Base   |                74M | Yes          |
| Small  |               244M | Yes          |
| Medium |               769M | Yes          |
| Large  |              1.55B | Yes          |
| Turbo  |               798M | Yes          |

The `turbo` model is an optimized version of the large model family intended for faster inference.

This gives researchers an additional experimental variable:

> **How does model size affect accuracy and computational cost?**

For example:

```text
Whisper Tiny
     ↓
Whisper Base
     ↓
Whisper Small
     ↓
Whisper Medium
     ↓
Whisper Large
     ↓
Whisper Turbo
```

---

# 4. Understanding SenseVoice

SenseVoice is designed as a broader speech-understanding system.

Its capabilities include:

* Automatic Speech Recognition
* Spoken Language Identification
* Speech Emotion Recognition
* Audio Event Detection

The SenseVoice project describes its broader work as covering more than 50 languages and training on more than 400,000 hours of data.

The architecture differs from Whisper.

The SenseVoice documentation describes SenseVoice as a non-autoregressive encoder-based model using SANM and CTC decoding.

A simplified representation is:

```text
Audio
  ↓
Acoustic Features
  ↓
Encoder
  ↓
CTC Decoding
  ↓
Transcription
```

This architectural difference is important when studying inference efficiency.

---

# 5. SenseVoice Is More Than ASR

One interesting aspect of SenseVoice is that transcription is only one of its capabilities.

The model family also supports speech understanding tasks such as:

```text
                 Audio
                   ↓
            SenseVoice
                   ↓
       ┌───────────┼───────────┐
       ↓           ↓           ↓
      ASR          SER         AED
       ↓           ↓           ↓
   Text         Emotion      Events
```

Where:

* **ASR** = Automatic Speech Recognition
* **SER** = Speech Emotion Recognition
* **AED** = Audio Event Detection

This makes SenseVoice particularly interesting for applications where speech recognition needs to be combined with other forms of audio understanding.

---

# 6. An Important Model-Selection Consideration

When designing an Indian-language experiment, the exact model checkpoint matters.

The broader SenseVoice research describes support for more than 50 languages.

However, the currently documented **SenseVoiceSmall released checkpoint** lists support for:

* Mandarin
* Cantonese
* English
* Japanese
* Korean

for its ASR and language-identification capabilities.

Therefore, a researcher should not automatically assume that the released SenseVoiceSmall checkpoint can be used for Marathi or Hindi.

Before running an Indian-language benchmark, the exact checkpoint and its documented language support must be verified.

This is an important example of why **research reproducibility requires precise model identification**.

---

# 7. Initial Comparison

At a high level:

| Feature                 | Whisper                     | SenseVoice                        |
| ----------------------- | --------------------------- | --------------------------------- |
| ASR                     | Yes                         | Yes                               |
| Multilingual speech     | Yes                         | Yes, depending on checkpoint      |
| Speech translation      | Yes                         | Not the primary focus             |
| Language identification | Yes                         | Yes                               |
| Emotion recognition     | Not a primary native output | Yes                               |
| Audio event detection   | Not a primary native output | Yes                               |
| Architecture            | Encoder-decoder Transformer | Non-autoregressive encoder / CTC  |
| Multiple model sizes    | Yes                         | Yes / model-family dependent      |
| Research focus          | Robust multilingual ASR     | Multilingual speech understanding |

The table should be treated as a high-level conceptual comparison rather than an experimental result.

---

# 8. Architecture Comparison

The architectural difference can be represented as:

```text
                 WHISPER

Audio
  ↓
Mel Spectrogram
  ↓
Encoder
  ↓
Decoder
  ↓
Autoregressive Tokens
  ↓
Text
```

Versus:

```text
               SENSEVOICE

Audio
  ↓
Acoustic Features
  ↓
Encoder
  ↓
CTC
  ↓
Text
```

These different approaches can produce different trade-offs between:

* accuracy
* latency
* memory
* decoding complexity
* scalability

That makes architecture an interesting variable for experimentation.

---

# 9. Autoregressive vs Non-Autoregressive Processing

Whisper uses an encoder-decoder sequence-to-sequence approach where the decoder generates tokens.

SenseVoice is described as non-autoregressive and uses CTC-based decoding.

Conceptually:

```text
Whisper

Token 1
   ↓
Token 2
   ↓
Token 3
   ↓
Token 4
   ↓
...


SenseVoice

Encoder Output
      ↓
CTC Decoding
      ↓
Output Sequence
```

This difference is one reason SenseVoice emphasizes low-latency inference.

The SenseVoice project reports significantly lower inference latency than comparable Whisper configurations in its own benchmark setup. Such numbers should not be interpreted as universal performance guarantees because hardware, implementation, model size, audio length, and benchmark conditions all affect latency.

---

# 10. Accuracy Is Not the Only Metric

Suppose an experiment produces:

```text
Whisper:
WER = 15%

SenseVoice:
WER = 17%
```

It would be tempting to conclude:

> Whisper is better.

But that conclusion may be incomplete.

Suppose:

```text
Whisper:
WER = 15%
RTF = 1.2

SenseVoice:
WER = 17%
RTF = 0.3
```

The practical choice could depend on the application.

For a real-time voice assistant, latency may matter greatly.

For offline research transcription, accuracy may be more important.

Therefore:

```text
Model Evaluation
       ↓
┌──────┼────────┬────────┐
↓      ↓        ↓        ↓
WER   CER      RTF     Memory
```

---

# 11. Word Error Rate

**Word Error Rate (WER)** is one of the most commonly used ASR metrics.

The standard formula is:

```text
WER = (S + D + I) / N
```

Where:

* S = substitutions
* D = deletions
* I = insertions
* N = number of words in the reference

Lower WER generally indicates better word-level transcription performance.

Example:

```text
Reference:
I am going to Pune

Prediction:
I am going Pune
```

The word:

> "to"

has been deleted.

This contributes to the WER.

---

# 12. Character Error Rate

**Character Error Rate (CER)** evaluates transcription differences at the character level.

It can be particularly useful when word segmentation or script-specific conventions make word-level comparison difficult.

A benchmark can therefore report:

```text
Model
  ↓
WER
+
CER
```

Using both metrics provides a broader view of transcription quality.

---

# 13. Indian-Language Benchmark Design

A proposed experiment could initially focus on:

```text
Hindi
Marathi
```

and compare:

```text
Whisper
vs
SenseVoice
```

However, before including a language, we must verify that the selected checkpoint supports it and that a suitable evaluation dataset is available.

This is particularly important for SenseVoice.

---

# 14. Proposed Experimental Dataset

The dataset should ideally contain:

* audio recordings
* reliable reference transcripts
* language labels
* speaker metadata
* recording information
* sufficient sample size
* appropriate licensing

The dataset should contain enough variation to allow meaningful analysis.

For example:

```text
                 Dataset
                    ↓
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Speakers     Regions     Conditions
        ↓           ↓           ↓
     Multiple    Multiple     Clean/Noise
```

---

# 15. Speaker-Independent Evaluation

Speaker overlap can create misleading results.

A better experimental design is:

```text
Training / Development
        ↓
Speaker A
Speaker B
Speaker C

Test
        ↓
Speaker D
Speaker E
Speaker F
```

The models should ideally be evaluated on speakers that were not used to develop or fine-tune the system.

This provides a better estimate of generalization.

---

# 16. Proposed Baseline Experiment

A practical first experiment could be:

### Languages

```text
Hindi
Marathi
```

### Models

```text
Whisper
SenseVoice
```

### Conditions

```text
Clean Speech
```

### Metrics

```text
WER
CER
Inference Time
RTF
Memory Usage
```

The first experiment should remain simple.

Only after the baseline is reliable should additional variables be introduced.

---

# 17. Experimental Matrix

The benchmark could eventually look like:

| Model      | Language | Condition | WER | CER | RTF | Memory |
| ---------- | -------- | --------- | --: | --: | --: | -----: |
| Whisper    | Hindi    | Clean     |   — |   — |   — |      — |
| Whisper    | Marathi  | Clean     |   — |   — |   — |      — |
| SenseVoice | Hindi    | Clean     |   — |   — |   — |      — |
| SenseVoice | Marathi  | Clean     |   — |   — |   — |      — |

The `—` values are intentionally left blank.

They should be filled only after actual experiments are conducted.

This is an important research principle:

> **Never present expected results as experimental results.**

---

# 18. Expanding the Experiment

Once the baseline is established:

```text
Phase 1
Clean Speech
        ↓
Phase 2
Additional Languages
        ↓
Phase 3
Background Noise
        ↓
Phase 4
Code-Switching
        ↓
Phase 5
Regional / Accent Variation
        ↓
Phase 6
Model Size Comparison
        ↓
Phase 7
Fine-Tuning
```

Each phase introduces additional complexity.

This makes it easier to determine which variable caused an observed performance change.

---

# 19. Noise Robustness

A useful experiment would introduce controlled background noise.

For example:

```text
Clean
  ↓
Low Noise
  ↓
Medium Noise
  ↓
High Noise
```

Then:

```text
Whisper
   ↓
WER at each noise level

SenseVoice
   ↓
WER at each noise level
```

The objective is to measure degradation.

A robust model should ideally maintain acceptable performance as noise increases.

---

# 20. Code-Switching

Indian speech frequently contains multiple languages within the same conversation.

For example:

> "आज माझी office मध्ये meeting आहे."

This could be classified as code-switched speech.

A benchmark could compare:

```text
Monolingual Speech
       vs
Code-Switched Speech
```

Potential metrics include:

* WER
* CER
* language identification
* error categories

The research question becomes:

> **How much does code-switching affect ASR performance?**

---

# 21. Marathi as a Focus Language

Marathi can be an especially interesting language for a focused study.

Potential research variables include:

* regional pronunciation
* speaker diversity
* Marathi-English code-switching
* recording environments
* technical vocabulary
* conversational speech

A focused experiment could therefore investigate:

> **How robust are multilingual ASR systems when recognizing Marathi speech under realistic conditions?**

This could eventually become a more substantial research project.

---

# 22. Hindi vs Marathi

A controlled Hindi-Marathi experiment could investigate whether the same ASR system behaves differently across the two languages.

For example:

```text
                 Same Model
                     ↓
          ┌──────────┴──────────┐
          ↓                     ↓
        Hindi                 Marathi
          ↓                     ↓
        WER                   WER
        CER                   CER
          ↓                     ↓
          └──────────┬──────────┘
                     ↓
                 Analysis
```

The goal should not simply be to identify which language has a lower score.

The deeper question is:

> **What factors could explain the difference?**

Potential factors include:

* dataset size
* speaker distribution
* language coverage in training
* transcription conventions
* domain mismatch
* accent
* code-switching
* vocabulary

---

# 23. Error Analysis

Numerical metrics should be supplemented with actual examples.

For every model, collect representative errors.

Possible categories include:

### Substitution

The model outputs the wrong word.

### Deletion

The model misses a word.

### Insertion

The model generates an additional word.

### Named Entity Error

Names and places are transcribed incorrectly.

### Code-Switching Error

The model struggles with mixed-language speech.

### Noise Error

Recognition quality deteriorates under noisy conditions.

### Technical Vocabulary Error

Domain-specific terms are incorrectly transcribed.

An error-analysis table could look like:

| Error Type      | Whisper | SenseVoice |
| --------------- | ------: | ---------: |
| Substitution    |       — |          — |
| Deletion        |       — |          — |
| Insertion       |       — |          — |
| Named Entity    |       — |          — |
| Code Switching  |       — |          — |
| Noise           |       — |          — |
| Technical Terms |       — |          — |

---

# 24. Computational Evaluation

The benchmark should measure more than recognition accuracy.

Important metrics include:

### Inference Time

How long does the model take to process the audio?

### Real-Time Factor

```text
RTF = Processing Time / Audio Duration
```

### Memory

How much RAM or VRAM does the model require?

### Model Size

How large are the model weights?

### Throughput

How many audio seconds can be processed per unit of time?

This gives us a more complete comparison.

---

# 25. Accuracy vs Latency

A practical visualization could look like:

```text
Accuracy
   ↑
   |
   |       ● Model A
   |
   |                ● Model B
   |
   |                         ● Model C
   |
   └──────────────────────────────→
             Latency
```

The ideal model depends on the application.

For:

### Offline transcription

Accuracy may be the primary concern.

### Real-time voice assistant

Latency becomes much more important.

### Edge deployment

Memory and compute requirements become critical.

---

# 26. Research Hypotheses

Once the benchmark methodology is established, hypotheses can be formulated.

For example:

### H1

> Modern multilingual ASR models will demonstrate different recognition performance across Indian languages.

### H2

> ASR performance will degrade as background noise increases.

### H3

> Code-switched speech will produce higher transcription error rates than comparable monolingual speech.

### H4

> Larger models will generally provide improved recognition accuracy but require greater computational resources.

### H5

> The relationship between model size and accuracy will vary across languages.

These hypotheses must be tested experimentally rather than assumed to be true.

---

# 27. From Comparison to Research

The goal of this work should not be:

> **Whisper vs SenseVoice: Which One Wins?**

That would make the project primarily an engineering benchmark.

A more research-oriented objective is:

> **Understanding how multilingual speech recognition architectures behave across diverse Indian-language conditions.**

The research process becomes:

```text
Models
  ↓
Benchmark
  ↓
Unexpected Results
  ↓
Error Analysis
  ↓
Hypothesis
  ↓
New Experiment
  ↓
Research Gap
  ↓
Potential Contribution
```

This is the transition from model comparison to research.

---

# 28. Possible Research Contributions

A future study could potentially contribute:

### Dataset Contribution

A carefully curated Indian-language evaluation dataset.

### Benchmark Contribution

A reproducible evaluation framework.

### Methodological Contribution

A better normalization or evaluation methodology.

### Model Adaptation

Language-specific adaptation or fine-tuning.

### Robustness Analysis

Evaluation under noise, accent, and code-switching.

### Error Taxonomy

A systematic classification of Indian-language ASR errors.

### Efficiency Analysis

Accuracy versus latency and memory trade-offs.

The actual contribution would depend on what the experiments reveal.

---

# 29. Reproducible Experiment Structure

A future implementation could use:

```text
asr-benchmark/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── metadata/
│
├── models/
│
├── configs/
│
├── src/
│   ├── preprocessing/
│   ├── inference/
│   ├── evaluation/
│   └── analysis/
│
├── experiments/
│
├── results/
│
├── notebooks/
│
├── reports/
│
├── requirements.txt
│
└── README.md
```

Each experiment can have its own configuration.

For example:

```text
EXP-001
Model: Whisper
Language: Marathi
Condition: Clean
Dataset: Dataset-X
```

and:

```text
EXP-002
Model: SenseVoice
Language: Marathi
Condition: Clean
Dataset: Dataset-X
```

This makes the comparison reproducible.

---

# 30. Proposed Research Roadmap

The complete research journey could be:

```text
Literature Review
       ↓
Model Selection
       ↓
Checkpoint Verification
       ↓
Dataset Selection
       ↓
Benchmark Infrastructure
       ↓
Baseline Experiment
       ↓
Hindi / Marathi Evaluation
       ↓
Additional Indian Languages
       ↓
Noise Experiments
       ↓
Code-Switching
       ↓
Accent / Regional Variation
       ↓
Error Analysis
       ↓
Statistical Evaluation
       ↓
Research Gap
       ↓
Proposed Improvement
       ↓
Validation
       ↓
Research Contribution
```

---

# 31. Important Research Principle

One of the most important lessons from this comparison is:

> **Never confuse a model's published benchmark with your own experimental result.**

For example, if SenseVoice's authors report better performance than Whisper on particular benchmark datasets, that does not automatically mean SenseVoice will perform better on Marathi speech.

Likewise, strong Whisper performance on multilingual datasets does not guarantee the best performance on every Indian language or every recording condition.

The correct approach is:

```text
Published Results
      ↓
Research Hypothesis
      ↓
Independent Experiment
      ↓
Your Results
      ↓
Analysis
```

---

# 32. Conclusion

Whisper and SenseVoice provide two interesting approaches to modern speech recognition and speech understanding.

Whisper offers a widely used multilingual, multitask encoder-decoder architecture with multiple model sizes and strong robustness-oriented research foundations.

SenseVoice takes a different approach, combining multilingual ASR with additional speech understanding capabilities and emphasizing efficient non-autoregressive inference.

For Indian-language research, however, the most important step is not to declare a winner before testing.

Instead, we should ask:

> **How do these models perform across Indian languages, speakers, environments, and linguistic conditions?**

A rigorous comparison should therefore measure:

```text
Accuracy
+
Robustness
+
Latency
+
Memory
+
Language Variation
+
Speaker Variation
+
Code-Switching
+
Error Patterns
```

The ultimate goal is to move from:

> **"Which model is better?"**

toward:

> **"Why does model performance change across languages and conditions, and how can we improve it?"**

That question provides a much stronger foundation for academic research.

---

# 33. Proposed Next Experiment

The first practical experiment for this research series should be deliberately small.

### Phase 1

```text
Models:
Whisper
SenseVoice

Languages:
Only languages supported by the exact selected checkpoints

Dataset:
One common evaluation dataset

Condition:
Clean speech

Metrics:
WER
CER
RTF
Memory
```

After verifying the baseline, the experiment can be expanded to:

```text
Phase 2 → More Indian languages
Phase 3 → Noise robustness
Phase 4 → Code-switching
Phase 5 → Regional variation
Phase 6 → Fine-tuning
```

This incremental design reduces experimental complexity and makes the results easier to interpret.

---

## Research Note

This article describes a proposed research comparison and does **not** present original experimental results.

Published model claims and benchmark results should be treated as background information. Any conclusions about Indian-language performance should be drawn only after running controlled experiments with documented datasets, model checkpoints, configurations, and evaluation procedures.

In particular, researchers should verify the exact SenseVoice checkpoint and its supported languages before including it in an Indian-language benchmark.

The objective of this research series is to build a **reproducible experimental framework for studying multilingual speech recognition**, rather than to assume the outcome in advance.
