# Fine-Tuning LLMs for Recipe Recommendation and Instruction Following

A comprehensive project exploring the fine-tuning of Large Language Models (LLMs) on recipe data to improve instruction-following capabilities while detecting and mitigating hallucinations in model outputs.

## 🎯 Project Overview

This project demonstrates how fine-tuning can transform a general-purpose LLM into a specialized recipe recommendation system that accurately follows nutritional constraints and user preferences. The key focus is on comparing baseline and fine-tuned model performance, with special attention to detecting and reducing hallucinations.

# Project Structure

Notebook Descriptions in `source` directory:

1. data-statistics.ipynb — Performs dataset exploration and visualization, including distributions of calories, protein, sodium, and ingredient frequencies.

2. data-generation.ipynb — Generates structured instruction–response pairs from the HUMMUS dataset for fine-tuning.

3. llm.ipynb — Fine-tunes the LLaMA 3.1 8B model using LoRA and QLoRA via the Unsloth library. Tracks training and validation loss on Weights & Biases.

4. evaluate.ipynb — Computes BLEU and ROUGE-L metrics, evaluates constraint satisfaction, and calculates hallucination rates through regex-based checks.

5. error-analysis.ipynb — Analyzes fine-tuned model errors, highlighting common hallucinations and their causes.

6. RAG.ipynb — Integrates a Retrieval-Augmented Generation system using ChromaDB and SentenceTransformer embeddings to ground outputs and reduce hallucinations.

## 📊 Dataset

**HUMMUS Dataset** - A comprehensive recipe collection featuring:
- Nutritional information (calories, protein, sodium, fiber)
- Preparation time and difficulty
- Health categories and dietary tags
- Ingredient lists and cooking instructions

## 🔍 Project Components

### 1. Dataset Exploration
- Statistical analysis of nutritional features (calories, protein, sodium)
- Distribution analysis of recipe duration (quick vs. long recipes)
- Health category frequency analysis
- Tag and ingredient popularity rankings
- Correlation studies (e.g., protein vs. calories)

### 2. Instruction Dataset Generation
Created diverse instruction-response pairs simulating real user queries:

**Example formats:**
```
Instruction: "Suggest a healthy pasta recipe under 400 calories."
Response: "Vegetarian Tomato Basil Pasta - 350 calories, 15g protein, ready in 20 minutes."

Instruction: "List three low-sodium chicken dishes."
Response: "1. Lemon Garlic Chicken (180mg sodium)... 2. Grilled Herb Chicken (200mg sodium)..."
```

Query types include:
- Ingredient-based filtering
- Nutritional constraints
- Preparation time requirements
- Health category specifications
- Multi-constraint combinations

### 3. Data Formatting
Structured dataset in JSONL format:
```json
{
  "instruction": "Suggest a vegan dessert under 300 calories",
  "input": "",
  "output": "Fruit Salad with Citrus Dressing - 250 calories, 5g fiber, 15 min preparation."
}
```

**Data Split:**
- Training: 80%
- Validation: 10%
- Test: 10%

### 4. Baseline Model Evaluation
- Utilized pre-trained models (GPT-2/LLaMA) without fine-tuning
- Evaluated on test queries to establish performance baseline
- Documented tendencies toward vague or incorrect responses

### 5. Fine-Tuning Implementation
- Models: GPT-2, Mistral, or LLaMA-2 with LoRA
- Training on instruction-response pairs
- Comprehensive tracking of:
  - Training/validation loss curves
  - Hyperparameters
  - Model checkpoints

### 6. Evaluation Framework

**Metrics:**
- **Relevance**: Constraint satisfaction accuracy
- **Factual Accuracy**: Verification against dataset entries
- **Fluency**: Response coherence and naturalness

**Measurement Methods:**
- BLEU/ROUGE scores for text similarity
- Precision/Recall for constraint satisfaction
- Human evaluation for fluency assessment

### 7. Hallucination Detection

**Types of Hallucinations:**
- **Factual**: Invented nutritional values not in dataset
- **Recipe**: Suggestions of non-existent recipes

**Detection Method:**
- Automated verification against dataset fields
- Hallucination rate calculation: `% of outputs with incorrect/fabricated facts`

### 8. Hallucination Mitigation Strategies

1. **Retrieval Augmentation**: Ground outputs in retrieved recipe data
2. **Post-Generation Verification**: Cross-check outputs against structured fields
3. **Prompt Engineering**: System-level constraints (e.g., "Only use facts from dataset")

