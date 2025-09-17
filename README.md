# Fine-grained Factual Consistency Evaluation for Large Language Models

A comprehensive system for evaluating the factual consistency of LLM outputs through atomic fact extraction and natural language inference verification.

## Overview

This project implements a multi-stage pipeline to assess factual accuracy in LLM-generated responses at a granular level. Instead of binary correct/incorrect classifications, the system identifies and categorizes different types of factual claims within generated text, providing nuanced insights into model reliability.

## Architecture

```
ASQA Dataset (948 samples)
         ↓
   Data Processing & Sampling
         ↓
   Selected Samples (400)
         ↓
   RAG Prompt Construction
   (Question + Knowledge Context)
         ↓
   LLM Response Generation
   (Mistral-7B-Instruct)
         ↓
   Atomic Fact Extraction
   (T5-Flan-Large)
         ↓
   Factual Verification
   (RoBERTa-Large-MNLI)
         ↓
   Statistical Analysis & Visualization
```

## Key Features

- **Multi-Model Pipeline**: Integrates three specialized models for generation, extraction, and verification
- **Atomic Fact Decomposition**: Breaks down responses into individually verifiable claims
- **Fine-grained Classification**: Categories facts as entailment, contradiction, neutral, or N/A
- **Confidence Analysis**: Quantifies model certainty and correlates with factual accuracy
- **Statistical Validation**: Comprehensive hypothesis testing and significance analysis

## Dataset

The project uses the **ASQA (Ambiguous Questions with Short Answers)** dataset:
- **Original size**: 948 samples with 6,368 expanded question-answer pairs
- **Final sample**: 400 strategically selected samples
- **Sampling strategy**: 100 no-knowledge cases, 300 knowledge-available cases

## Models Used

### 1. Text Generation
- **Model**: Mistral-7B-Instruct-v0.1
- **Purpose**: RAG-based response generation using Wikipedia context
- **Configuration**: Float16, device mapping for GPU optimization

### 2. Atomic Fact Extraction
- **Model**: T5-Flan-Large
- **Purpose**: Decompose responses into atomic, verifiable facts
- **Prompt Engineering**: Few-shot prompting with structured examples

### 3. Factual Verification
- **Model**: RoBERTa-Large-MNLI
- **Purpose**: Natural Language Inference for fact-checking
- **Output**: Entailment, neutral, or contradiction with confidence scores

## Results

### Distribution of Verification Results
- **Neutral**: 43.5% (174 cases) - Neither clearly correct nor incorrect
- **Entailment**: 28.7% (115 cases) - Factually supported by context
- **Contradiction**: 11.2% (45 cases) - Factually incorrect
- **N/A**: 16.5% (66 cases) - Unverifiable claims

### Confidence Analysis
| Category | Mean Confidence | Std Dev |
|----------|----------------|---------|
| Entailment | 0.816 | ±0.12 |
| Neutral | 0.827 | ±0.13 |
| Contradiction | 0.726 | ±0.15 |

### Statistical Significance
- **ANOVA**: F(2,307) = 7.36, p < 0.001
- **Kruskal-Wallis**: H = 13.17, p = 0.001
- **Post-hoc (Dunn's test)**: Significant differences between contradiction and other categories


## Key Findings

1. **Confidence as Predictor**: Model confidence scores partially predict factual accuracy, with contradictions showing significantly lower confidence.

2. **Neutral Dominance**: Nearly half of all responses fall into the "neutral" category, suggesting many LLM outputs contain ambiguous or unverifiable information.

3. **Error Detection**: The system successfully identifies factual contradictions while maintaining high precision.

4. **Statistical Validation**: All major findings are statistically significant with proper multiple comparison corrections.
