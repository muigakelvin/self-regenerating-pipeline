Here’s your **clean, professional, fully de-personalized project documentation** — no names, no social links, and generalized beyond Yelp 👇

---

# Self-Healing Data Pipeline

A production-ready, AI-powered data pipeline that automatically detects and heals data quality issues during sentiment analysis using Apache Airflow and a local LLM.

![Python](https://img.shields.io/badge/Python-3.12+-blue?logo=python)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-3.0+-017CEE?logo=apache-airflow)
![Ollama](https://img.shields.io/badge/Ollama-LLaMA%203.2-orange)
![License](https://img.shields.io/badge/License-MIT-green)

---

## Table of Contents

* Overview
* Architecture
* Features
* Self-Healing Capabilities
* Prerequisites
* Installation
* Configuration
* Usage
* Pipeline Tasks
* Health Monitoring
* Output
* Project Structure

---

## Overview

Traditional data pipelines often fail when encountering malformed, missing, or unexpected data.

This project demonstrates an **agentic, self-healing pipeline design** where the system:

1. **Diagnoses** data quality issues in real time
2. **Automatically repairs** problematic records
3. **Performs sentiment analysis** using a local language model
4. **Generates health metrics** for observability and monitoring

---

## Architecture

![Self healing pipeline](assets/Self%20healing%20pipeline.jpeg)

---

## Features

| Feature              | Description                                             |
| -------------------- | ------------------------------------------------------- |
| Self-Healing         | Automatically fixes missing, malformed, or invalid data |
| Local LLM            | Runs inference locally without external API calls       |
| Batch Processing     | Supports configurable batch sizes and offsets           |
| Parallel Execution   | Enables concurrent DAG execution                        |
| Health Monitoring    | Tracks pipeline health and processing quality           |
| Graceful Degradation | Falls back safely when inference fails                  |
| Observability        | Produces detailed metrics and statistics                |

---

## Self-Healing Capabilities

The pipeline detects and fixes common data quality issues:

| Error Type        | Detection                | Healing Action           |
| ----------------- | ------------------------ | ------------------------ |
| `missing_text`    | Field is null            | Replace with placeholder |
| `empty_text`      | Empty or whitespace      | Replace with placeholder |
| `wrong_type`      | Not a string             | Convert to string        |
| `invalid_content` | No meaningful characters | Replace with marker      |
| `too_long`        | Exceeds max length       | Truncate safely          |

---

## Prerequisites

* Python 3.12+
* Local LLM runtime (e.g., Ollama)
* Apache Airflow 3.0+
* Recommended: 8GB+ RAM for model inference

---

## Installation

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd <repo-name>
```

---

### 2. Create virtual environment

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
```

---

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

### 4. Start local LLM service

```bash
ollama serve
ollama pull llama3.2
```

---

### 5. Initialize Airflow

```bash
export AIRFLOW_HOME=$(pwd)
airflow db migrate
```

---

### 6. Start Airflow

```bash
airflow standalone
```

---

## Configuration

### Environment Variables

| Variable                   | Default                  | Description        |
| -------------------------- | ------------------------ | ------------------ |
| `PIPELINE_BASE_DIR`        | Project root             | Base directory     |
| `PIPELINE_INPUT_FILE`      | `input/data.json`        | Input dataset      |
| `PIPELINE_OUTPUT_DIR`      | `output/`                | Output directory   |
| `PIPELINE_MAX_TEXT_LENGTH` | `2000`                   | Max text length    |
| `OLLAMA_HOST`              | `http://localhost:11434` | LLM endpoint       |
| `OLLAMA_MODEL`             | `llama3.2`               | Model name         |
| `OLLAMA_TIMEOUT`           | `120`                    | Timeout in seconds |
| `OLLAMA_RETRIES`           | `3`                      | Retry attempts     |

---

### DAG Parameters

```json
{
  "input_file": "/path/to/data.json",
  "batch_size": 100,
  "offset": 0,
  "ollama_model": "llama3.2"
}
```

---

## Usage

### Trigger via Airflow UI

1. Open `http://localhost:8080`
2. Locate the pipeline DAG
3. Trigger with custom parameters
4. Monitor execution

---

### Trigger via CLI

```bash
airflow dags trigger self_healing_pipeline \
  --conf '{"batch_size": 100, "offset": 0}'
```

---

### Batch Processing

```bash
python scripts/batch_runner.py --total 5000000 --batch-size 1000
python scripts/batch_runner.py --total 5000000 --batch-size 5000 --parallel 5
python scripts/batch_runner.py --total 5000000 --batch-size 1000 --start 100000
python scripts/batch_runner.py --total 5000000 --batch-size 10000 --dry-run
```

---

## Pipeline Tasks

| Task                    | Description              |
| ----------------------- | ------------------------ |
| load_model              | Initialize model         |
| load_data               | Read dataset             |
| diagnose_and_heal_batch | Detect & fix issues      |
| analyze_sentiment       | Run LLM inference        |
| aggregate_results       | Compute metrics          |
| generate_health_report  | Evaluate pipeline health |

---

## Health Monitoring

Pipeline health is classified as:

| Status   | Condition                |
| -------- | ------------------------ |
| HEALTHY  | Minimal issues detected  |
| WARNING  | High healing rate        |
| DEGRADED | Some failures occurred   |
| CRITICAL | Significant failure rate |

---

### Example Report

```json
{
  "pipeline": "self_healing_pipeline",
  "health_status": "HEALTHY",
  "metrics": {
    "total_processed": 100,
    "success_rate": 0.85,
    "healing_rate": 0.15,
    "degradation_rate": 0.0
  }
}
```

---

## Output

Generated results are stored in:

```
output/
└── sentiment_analysis_summary_<timestamp>.json
```

---

### Output Schema

```json
{
  "run_info": {...},
  "totals": {...},
  "rates": {...},
  "results": [...]
}
```

---

## Project Structure

```
project-root/
├── dags/
│   └── pipeline_dag.py
├── scripts/
│   └── batch_runner.py
├── input/
├── output/
├── logs/
├── requirements.txt
└── README.md
```

---

## Notes

* Designed for **resilient data engineering workflows**
* Suitable for **large-scale batch processing**
* Can be extended with:

  * data validation frameworks (e.g., Great Expectations)
  * streaming systems (Kafka, Flink)
  * lakehouse storage (Delta Lake / Iceberg)

---
