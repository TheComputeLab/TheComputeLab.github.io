---
title: "Word Error Rate (WER) Explained"
weight: 7
date: 2026-08-22
draft: false
description: "A practical and research-oriented guide to Word Error Rate and its use in automatic speech recognition evaluation."
tags:
- Word Error Rate
- WER
- Speech Recognition
- ASR
- Indian Languages
- Marathi
- Hindi
- Machine Learning
- Evaluation
- Research
categories:
- Research
- Speech AI
---


## Introduction

**Word Error Rate (WER)** is one of the most widely used metrics for evaluating Automatic Speech Recognition (ASR). It compares a reference transcription with a model's hypothesis.

The basic formula is:

```text
WER = (S + D + I) / N
```

where `S` is substitutions, `D` is deletions, `I` is insertions, and `N` is the number of words in the reference.

## 1. The Three Error Types

### Substitution

Reference:

```text
I live in Pune
```

Hypothesis:

```text
I live in Mumbai
```

`Pune → Mumbai` is a substitution.

### Deletion

Reference:

```text
I live in Pune
```

Hypothesis:

```text
I live Pune
```

`in` is a deletion.

### Insertion

Reference:

```text
I live in Pune
```

Hypothesis:

```text
I live in the Pune
```

`the` is an insertion.

## 2. Example

If the reference contains five words and the model makes one deletion:

```text
WER = 1 / 5
    = 20%
```

WER can exceed 100% when there are many insertions.

## 3. WER for Indian Languages

Indian-language evaluation requires consistent:

- Unicode normalization
- punctuation handling
- tokenization
- numeral normalization
- whitespace rules
- script handling

For multilingual or code-switched speech, the same normalization policy must be applied to both reference and hypothesis.

## 4. WER and Code-Switching

Consider:

```text
मला आज office मध्ये meeting आहे
```

A benchmark should define how Marathi and English tokens are handled before calculating WER.

```text
Reference
   ↓
Normalization
   ↓
Tokenization
   ↓
Hypothesis
   ↓
Normalization
   ↓
Alignment
   ↓
WER
```

## 5. Limitations

WER does not directly measure semantic similarity and can be sensitive to tokenization and normalization. It should therefore be combined with CER and error analysis.

## 6. Research Use

A strong benchmark should report WER by:

- language
- speaker
- domain
- noise condition
- code-switching condition

The deeper research question is not simply which model has the lowest WER, but **why performance differs across languages and conditions**.

## Conclusion

WER is a foundational ASR metric. For Indian-language research, it should be used as part of a broader framework containing CER, robustness testing, statistical analysis, and reproducibility.

