<![CDATA[<div align="center">

# 🧠 Crypto Intelligence Terminal

### Self-Hosted AI Trading Intelligence — Powered by Open-Source LLMs

[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)](https://python.org)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![Mistral 7B](https://img.shields.io/badge/LLM-Mistral--7B-FF6F00?logo=huggingface&logoColor=white)](https://huggingface.co/mistralai/Mistral-7B-v0.1)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

*Analyze sentiment · Predict prices · Generate actionable BUY / SELL / HOLD signals — all running locally on your hardware.*

---

</div>

## 📌 Overview

The **Crypto Intelligence Terminal** is a fully self-hosted, hybrid-compute AI pipeline for cryptocurrency sentiment analysis and price prediction. It combines traditional quantitative modeling (XGBoost, Prophet, LSTM) with state-of-the-art generative AI (Mistral-7B via QLoRA fine-tuning) to deliver an end-to-end framework for data-driven trading decisions — with zero reliance on expensive third-party APIs or cloud-hosted foundation models.

> **Why self-hosted?** Full data privacy, no recurring API costs, complete control over model behavior, and the freedom to fine-tune on your own market thesis.

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 📡 **Real-Time Data Ingestion** | Continuous streams from Binance (OHLCV), NewsAPI, Reddit (`r/cryptocurrency`), and Etherscan (whale transactions). |
| 🤖 **AI-Powered Sentiment Analysis** | Locally hosted Ollama Mistral-7B + FinBERT for deep contextual understanding of market narratives. |
| 📈 **Multi-Model Price Prediction** | Ensemble of Prophet (trend decomposition), LSTM (temporal memory), and XGBoost (gradient boosting). |
| 🎯 **Intelligent Signal Generation** | Definitive BUY / SELL / HOLD signals with confidence scores and explainability metrics. |
| 🔄 **Backtesting Engine** | Validates historical accuracy using Sharpe Ratio, directional accuracy, and drawdown analysis. |
| 📊 **Unified Dashboards** | Animated web dashboard (HTML/CSS/JS + Lightweight Charts) and CLI-based rich terminal UI. |
| 🏗️ **Fully Containerized** | Single-command Docker Compose deployment with automatic GPU/CPU fallback. |

---

## 🏛️ System Architecture

```mermaid
flowchart TD
    subgraph DataSources["📡 Data Sources"]
        direction LR
        A["Binance API\nPrice OHLCV"]
        B["NewsAPI\nCrypto News"]
        C["Reddit API\nr/cryptocurrency"]
        D["Etherscan API\nWhale Txns"]
    end

    subgraph Backend["⚙️ Python Backend - FastAPI"]
        E["Data Ingestion\nPipeline"]
        F[("PostgreSQL +\nTimescaleDB")]
        G["Sentiment Engine\nFinBERT"]
        H["Price Prediction\nProphet + XGBoost"]
        I["Signal Generator\nBUY / SELL / HOLD"]
        J["Backtesting Engine\nSharpe Ratio"]
        K["Ollama / Mistral 7B\nAI Insights"]

        E --> F
        F --> G
        F --> H
        G --> I
        H --> I
        I --> J
    end

    subgraph Frontend["🖥️ Frontend Dashboard"]
        L["HTML / CSS / JS\nApexCharts"]
    end

    A --> E
    B --> E
    C --> E
    D --> E

    F --> L
    G --> L
    H --> L
    I --> L
    J --> L
    K --> L
```

---

## 📋 System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **Python** | 3.9+ | 3.11+ |
| **Docker** | Docker Engine + Compose | Docker Desktop (latest) |
| **System RAM** | 16 GB | 32 GB |
| **GPU (Optional)** | — | NVIDIA GPU with 8 GB+ VRAM |

> A GPU is optional but strongly recommended for QLoRA fine-tuning and accelerated Mistral-7B inference. The system automatically falls back to CPU if no compatible GPU is detected.

---

## 🚀 Quick Start

### 1. Clone & Configure

```bash
git clone https://github.com/MasterManoj9/Crypto_terminal.git
cd Crypto_terminal
cp .env.example .env
```

Open `.env` and insert your API keys:

```env
BINANCE_API_KEY=your_binance_api_key
BINANCE_API_SECRET=your_binance_secret
NEWS_API_KEY=your_newsapi_key
REDDIT_CLIENT_ID=your_reddit_client_id
REDDIT_CLIENT_SECRET=your_reddit_secret
ETHERSCAN_API_KEY=your_etherscan_key
```

### 2. Deploy

```bash
docker-compose up -d --build
```

Open **`http://localhost:8501`** — no login required.

### 3. Platform Scripts (Alternative)

```powershell
# Windows
./scripts/deploy_model.ps1

# Linux / macOS
./scripts/deploy_model.sh
```

---

## 🧹 Teardown

```bash
# Stop containers
docker-compose down

# Full reset (removes images + volumes)
docker-compose down --rmi all -v
```

---

## 🔬 Prediction Pipeline

### Stage 1 — Data Ingestion
Aggregates OHLCV prices, news, whale transactions, and Reddit posts into PostgreSQL/TimescaleDB.

### Stage 2 — Sentiment Extraction
FinBERT + Mistral-7B extract Bullish/Bearish sentiment from textual feeds.

### Stage 3 — Price Prediction
Prophet and XGBoost analyze historical data with technical indicators (MA, RSI, Bollinger Bands).

### Stage 4 — Signal Generation
Cross-references sentiment + predictions → BUY / SELL / HOLD with confidence scores.

### Stage 5 — Backtesting
Validates signals against history using Sharpe Ratio \& win rate.

### Stage 6 — Dashboard
Live animated frontend via Lightweight Charts + ApexCharts.

---

## ⚡ GPU & CPU Runtime

| Component | Runtime | Rationale |
|-----------|---------|-----------|
| Mistral-7B Inference | **GPU** | Compute-bound token generation |
| RAG Retrieval | **CPU** | I/O-bound vector queries |
| FinBERT | **Auto** | Lightweight — runs on either |
| XGBoost / Prophet | **CPU** | Negligible GPU benefit |

Automatic fallback: probes `/api/v1/model/runtime` → restarts on CPU if no CUDA detected.

---

## 🧬 Mistral-7B Optimizations

- **4-Bit Quantization** — NF4 via `bitsandbytes`, ~75% VRAM reduction
- **LoRA** — Trainable rank-decomposition matrices, 10–100× faster fine-tuning
- **Split RAG** — GPU for generation, CPU for retrieval

---

## 📊 Benchmarks (2026-04-02)

### Aggregate

| Model | Accuracy | Sharpe | Win Rate | Trades |
|-------|:--------:|:------:|:--------:|:------:|
| **CPU** (GBR) | **52.44%** | **4.54** | **30.23%** | 210 |
| GPU (LSTM) | 49.44% | -1.39 | 1.43% | 8 |

### Per Asset

| Asset | CPU Acc | CPU Sharpe | CPU WR | GPU Acc | GPU Sharpe | GPU WR |
|:-----:|:-------:|:----------:|:------:|:-------:|:----------:|:------:|
| BTC | 52.08% | 1.81 | 29.54% | 51.26% | 0.00 | 0.00% |
| ETH | 53.89% | 7.80 | 33.86% | 50.28% | 0.00 | 0.00% |
| DOGE | 50.28% | 3.48 | 24.33% | 44.13% | -6.95 | 7.14% |

---

## 📐 Mathematical Foundations

### Ensemble Consensus
$$P_{\text{ensemble}}(c) = 0.3 \cdot P_{\text{prophet}}(c) + 0.4 \cdot P_{\text{lstm}}(c) + 0.3 \cdot P_{\text{xgb}}(c)$$

### Prophet Decomposition
$$y(t) = g(t) + s(t) + h(t) + \epsilon_t$$

### LSTM Gates
$$f_t = \sigma(W_f \cdot [h_{t-1}, x_t] + b_f)$$
$$C_t = f_t \odot C_{t-1} + i_t \odot \tanh(W_C \cdot [h_{t-1}, x_t] + b_C)$$

### XGBoost Objective
$$\mathcal{L}(\phi) = \sum_{i} l(\hat{y}_i, y_i) + \gamma T + \tfrac{1}{2}\lambda \sum w_j^2$$

### QLoRA Adaptation
$$W_{\text{adapted}} = W_{\text{NF4}} + (A \times B) \cdot \tfrac{\alpha}{r}$$

---

## 🤝 Contributing

Open an issue → discuss → submit PR against `main`.

## 📄 License

[MIT License](LICENSE)

---

<div align="center">

**Built with ❤️ for the open-source crypto community.**

</div>
]]>
