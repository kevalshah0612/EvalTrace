# EvalTrace

RAG Evaluation Harness with CI/CD Quality Gates.

## Overview

EvalTrace is an automated evaluation system for RAG pipelines. It runs test cases against FilingQuery and measures answer quality using evaluation metrics such as answer relevancy, faithfulness, contextual recall, and hallucination.

The main purpose of EvalTrace is to turn AI quality into something measurable and enforceable in CI/CD.

## Why this project

Most RAG applications look good in demos but fail quietly after code changes. EvalTrace solves that problem by running automated evaluations on every push or pull request and blocking merges when quality drops below defined thresholds.

This project is useful because it demonstrates:
- Evaluation-driven LLM engineering
- Quality gates in CI/CD
- Repeatable benchmarks with golden datasets
- Before vs. after experiments with real metrics

## Features

- Runs automated evaluation on FilingQuery
- Supports golden dataset benchmarking
- Uses DeepEval metrics for quality measurement
- Blocks merges when thresholds are breached
- Stores baseline and final result files
- Can be extended to evaluate ReviewBot outputs
- Integrates directly with GitHub Actions

## Evaluation Metrics

- Answer Relevancy
- Faithfulness
- Contextual Recall
- Contextual Precision
- Hallucination

## Architecture

```text
[GitHub Actions Trigger]
        │
        ▼
[pytest + DeepEval Test Suite]
        │
        ▼
[eval_dataset.json]
        │
   Pass │ Fail
        │
        ├── Continue pipeline
        └── Block merge
```

## Tech Stack

- Python
- DeepEval
- pytest
- GitHub Actions
- GPT-4o-mini via OpenAI API
- FilingQuery
- ReviewBot

## Project Structure

```text
evaltrace/
├── README.md
├── tests/
│   ├── test_rag.py
│   └── test_reviewbot.py
├── eval_dataset.json
├── results/
│   ├── baseline_eval.json
│   └── final_eval.json
├── .github/
│   └── workflows/
│       └── eval.yml
└── requirements-eval.txt
```

## Getting Started

### Prerequisites

- Python 3.11
- OpenAI API key
- Confident AI API key
- A running FilingQuery API

### Install

```bash
git clone https://github.com/your-username/evaltrace.git
cd evaltrace
python -m venv .venv
source .venv/bin/activate
pip install -r requirements-eval.txt
```

### Run evaluations

```bash
deepeval test run tests/test_rag.py
```

### Save baseline and final experiments

```bash
deepeval test run tests/test_rag.py --output results/baseline_eval.json
deepeval test run tests/test_rag.py --output results/final_eval.json
```

## CI/CD Quality Gates

EvalTrace is designed to run inside GitHub Actions. If evaluation metrics fail the configured thresholds, the workflow exits with failure and blocks the merge.

Example threshold goals:
- Answer relevance >= 0.80
- Faithfulness >= 0.85
- Context recall >= 0.75
- Hallucination <= 0.10

## Dataset

The evaluation dataset is a version-controlled benchmark of 500+ question-answer-context examples. It should stay fixed across runs so results remain comparable over time.

## Roadmap

- [ ] Build 500+ golden QA pairs
- [ ] Add FilingQuery evaluation suite
- [ ] Add ReviewBot summary evaluation suite
- [ ] Wire tests into GitHub Actions
- [ ] Save baseline and final experiment outputs
- [ ] Publish before/after quality table

## Demo

TODO:
- Add CI workflow screenshot
- Add blocked-merge example
- Add sample evaluation output
- Add before/after metrics table

## Related Projects

- FilingQuery — evaluated RAG pipeline
- ReviewBot — second pipeline evaluated by EvalTrace

## License

MIT
