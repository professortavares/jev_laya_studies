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

## Architecture

- `main.py` — Entry point for scripts
- `notebooks/` — Jupyter notebooks for exploration and analysis
- `.env` — Local environment variables (API keys, config)
- `uv.lock` — Frozen dependency versions (commit this)
