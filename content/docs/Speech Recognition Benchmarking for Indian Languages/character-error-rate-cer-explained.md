---
title: "Character Error Rate (CER) Explained"
weight: 8
date: 2026-08-22
draft: false
description: "A practical guide to Character Error Rate, its calculation, relationship with WER, and importance for Indian-language ASR."
tags:
- Character Error Rate
- CER
- Speech Recognition
- ASR
- Indian Languages
- Marathi
- Hindi
- Evaluation
- Machine Learning
- Research
categories:
- Research
- Speech AI
---

## Introduction

**Character Error Rate (CER)** evaluates ASR output at the character level rather than the word level.

```text
CER = (S + D + I) / N
```

where the edit operations and `N` refer to characters.

## 1. CER vs WER

```text
WER → word-level errors
CER → character-level errors
```

Both can reveal different aspects of transcription quality.

## 2. Why CER Matters for Indian Languages

CER can be useful for languages using scripts such as:

```text
Devanagari
Bengali
Tamil
Telugu
Kannada
Malayalam
Gujarati
Gurmukhi
```

Word boundaries and tokenization conventions can vary, while character-level analysis provides a more fine-grained view.

## 3. Unicode Normalization

Indian-language evaluation must define:

- Unicode normalization
- combining-character handling
- punctuation
- whitespace
- numerals
- special symbols

The exact same normalization must be applied to reference and hypothesis.

## 4. Transliteration

Consider:

```text
मला उद्या पुण्याला जायचं आहे.
```

versus:

```text
mala udya punyala jaycha aahe
```

A direct CER comparison treats these as different scripts. A research benchmark must decide whether transliteration is an error or a separate representation.

## 5. CER and Code-Switching

For:

```text
मला आज office मध्ये meeting आहे
```

the evaluation pipeline should consistently process both scripts.

## 6. Limitations

CER does not directly measure semantic correctness, grammar, or meaning preservation. A transcript can have low CER and still contain an important semantic error.

## 7. WER + CER

A useful benchmark reports both:

| Model | Language | WER | CER |
|---|---|---:|---:|
| Model A | Hindi | — | — |
| Model A | Marathi | — | — |
| Model B | Hindi | — | — |
| Model B | Marathi | — | — |

## Conclusion

CER complements WER and is especially valuable when evaluating multilingual Indian-language ASR, transliteration, code-switching, and script-specific behavior.

