# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Research project for studying Jev and Laya AI models. Combines Python scripts with Jupyter notebooks for interactive exploration and analysis. Uses Typesafe SDK for model API access.

## Development Setup

**Environment Setup**
- Python 3.12 required (specified in `.python-version`)
- Uses `uv` package manager (faster, simpler alternative to pip)
- Install dependencies: `uv sync`
- Virtual environment at `.venv/` (created by uv)

**Environment Variables**
- Copy `.env.example` to `.env`
- Add your `TYPESAFE_API_KEY` to enable Typesafe SDK calls
- `.env` is git-ignored, never commit credentials

## Running Code

**Run Main Script**
```bash
uv run python main.py
```

**Run Jupyter Notebooks**
```bash
uv run jupyter lab
```
- Notebooks stored in `notebooks/` directory
- Dev dependency `jupyterlab` with `ipykernel` kernel for execution

**Install New Dependencies**
```bash
uv add package-name           # Add to dependencies
uv add -d package-name        # Add to dev dependencies
uv sync                       # Update lock file and environment
```

## Key Dependencies

- **ML/Models**: transformers, laya, huggingface-hub, accelerate
- **Data**: pandas, scikit-learn
- **Visualization**: matplotlib, seaborn
- **API**: typesafe-sdk
- **Config**: python-dotenv

## Notebooks Overview

### 01_jev_introduction.ipynb
Introduction to Jev model from Typesafe. Demonstrates:
- Three response types: Choice (categorical), Score (numeric scale), Noul (probability 0-1)
- Confidence scores and probability distributions
- Token efficiency for cost optimization
- Multilingual support
- Example: support ticket routing analysis

### 02_jev_tweets_classification.ipynb
Disaster tweets classification task. Demonstrates:
- Feature engineering and data exploration
- Batch processing strategy (32-tweet batches) for API efficiency
- Baseline comparison: LogisticRegression (80.1% accuracy) vs Jev zero-shot (71%)
- ROC-AUC metrics and threshold optimization
- Error analysis and prediction confidence

### 03_jev_tweets_classification_explainability.ipynb
Structured explainability for tweet classification. Demonstrates:
- Decomposing model decisions into interpretable factors
- Multi-question analysis using Jev
- Confidence-based explanation generation
- Trade-offs between accuracy metrics and explainability

### 04_jev_titanic_classification.ipynb
Largest analysis: supervised vs zero-shot paradigms. Demonstrates:
- LogisticRegression (supervised): OneHotEncoder + StandardScaler pipeline
- Jev (zero-shot): semantic state representation (numeric→text features)
- Batch classification with structured explainability
- 5 interpretable factors per sample: demographic, class, family, economic, overall
- Threshold-based explanation (0.7=favorable, 0.3=unfavorable)
- Correlation analysis between model outputs and explanatory factors

### 06_jev_openai_collab.ipynb
Multi-agent iterative refinement: OpenAI proposes, Jev evaluates, feedback drives next iteration. Demonstrates:
- Closed-loop collaboration between specialized models
- Structured evaluation: 5 binary criteria (correctness, completeness, feasibility, clarity, simplicity) scored by Jev
- Convergence detection: stop when all criteria ≥ 0.80 confidence
- Feedback loop: failing criteria → actionable feedback → OpenAI refinement
- Semantic state representation of architecture problem in Portuguese
- Convergence table: track scores per round until convergence

## Key Analysis Patterns

**Batch Processing**
- Use batch_size=32 for API cost efficiency
- Multiple samples per call reduces token usage

**Semantic State Representation**
- Convert numeric features to natural language for Jev understanding
- Example: Age + class + family status → descriptive text

**Structured Explainability**
- Query decomposed factors separately (5+ questions per sample)
- Build explanations from factor probabilities
- More interpretable than single confidence score

**Zero-shot vs Supervised**
- Zero-shot (Jev): better explainability, lower metrics
- Supervised (LogisticRegression): better metrics, less interpretable
- Choose based on use case requirements

**Iterative Multi-Agent Collaboration**
- Use Jev as evaluator: binary judgments → confidence scores
- Use OpenAI as proposer: generate or refine solutions based on feedback
- Convergence criteria: all evaluation dimensions meet threshold
- Feedback loop: failing criteria → structured feedback → next iteration
- Stops at convergence or max_rounds, whichever comes first

## Architecture

- `main.py` — Entry point for scripts
- `notebooks/` — Jupyter notebooks for exploration and analysis
  - Each notebook extensively documented with line comments and markdown sections
  - Covers different analysis methodologies and model comparisons
- `.env` — Local environment variables (API keys, config)
- `uv.lock` — Frozen dependency versions (commit this)
