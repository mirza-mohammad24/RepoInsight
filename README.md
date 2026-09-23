<div align="center">

# 🔍 RepoInsight AI

**Intelligent Repository Health Analysis & Contribution Impact Prediction using Graph Machine Learning**

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688.svg)](https://fastapi.tiangolo.com/)
[![PyTorch](https://img.shields.io/badge/PyTorch-GraphSAGE-EE4C2C.svg)](https://pytorch.org/)
[![React 18](https://img.shields.io/badge/Frontend-React%2018%20%7C%20Vite-61DAFB.svg)](https://react.dev/)
[![Tailwind CSS](https://img.shields.io/badge/UI-Tailwind%20CSS-38B2AC.svg)](https://tailwindcss.com/)
[![Cytoscape.js](https://img.shields.io/badge/Graph-Cytoscape.js-EA580C.svg)](https://js.cytoscape.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

<p align="center">
  <a href="#-problem-statement">Problem Statement</a> •
  <a href="#-key-features">Key Features</a> •
  <a href="#-system-architecture">Architecture</a> •
  <a href="#-mathematical-foundations">Math Foundations</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-api-reference">API Reference</a> •
  <a href="#-project-roadmap">Roadmap</a>
</p>

</div>

---

## 📌 Problem Statement

Software codebases grow exponentially in complexity over time. Engineering teams and open-source maintainers frequently struggle with invisible technical debt:

* **Isolated Code Quality Analysis**: Standard linters (ESLint, Flake8) and static analyzers (SonarQube) evaluate files in silos without factoring in **temporal Git evolution** or **topological dependency ripple**.
* **Fragile Hotspots**: Highly complex files that undergo frequent modifications are statistical breeding grounds for regressions.
* **Knowledge Silos (Bus Factor Risk)**: Critical subsystems authored almost exclusively by one contributor create catastrophic single-points-of-failure if that contributor departs.
* **Blind Pull Request Merging**: Reviewers cannot reliably estimate the downstream **blast radius** of a proposed change across imported modules.

**RepoInsight AI** unifies **Git commit mining**, **polyglot Abstract Syntax Tree (AST) analysis**, and **Graph Neural Networks (GraphSAGE)** to score codebase maintainability, pinpoint structural vulnerabilities, and predict pull request ripple risk before code is merged.

---

## ✨ Key Features

### 1. 📊 Multi-Pillar Codebase Health Engine
Aggregates repository health into a calibrated **0 to 100 Maintainability Index** derived from 4 foundational pillars:
* **Code Complexity (25%)**: McCabe cyclomatic complexity evaluated across functions and classes.
* **Code Churn & Velocity (25%)**: Historical modification frequencies, addition/deletion ratios, and commit velocity.
* **Knowledge Ownership (25%)**: Avelino/Rigby knowledge distribution, Bus Factor computation, and team contribution Gini inequality.
* **Coupling & Architecture (25%)**: Robert C. Martin instability metrics, cyclic import detection, and module dependency density.

### 2. 🎯 Adam Tornhill Hotspot Matrix
Pinpoints critical architectural debt by mapping files onto a bivariate plane of **Cyclomatic Complexity $\times$ Code Churn**. Files occupying the top-right quadrant are flagged as priority refactoring targets.

### 3. 🕸️ Interactive Force-Directed Dependency Topology
Visualizes the multi-language import graph in real-time using **Cytoscape.js** with a force-directed `cose` layout:
* Node sizing calibrated to architectural importance (In-degree & PageRank).
* Real-time inspection sidebar displaying SLOC, cyclomatic complexity, afferent/efferent coupling, and instability.
* Search and highlighting for immediate neighbor exploration.

### 4. 🧠 Deep Graph Neural Network (PyTorch GraphSAGE)
* Operates directly on the directed dependency topology $\mathcal{G} = (\mathcal{V}, \mathcal{E})$.
* Employs inductive neighborhood aggregation to learn structural embeddings for every source file.
* Multi-task prediction heads forecast continuous change impact (blast radius magnitude) and multi-class regression risk (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`).

### 5. 🧪 Interactive Pull Request Impact Simulator
Allows developers to select files touched in a hypothetical or active Pull Request:
* Performs a **reverse Breadth-First Search (BFS)** to identify the exact downstream blast radius.
* Evaluates risk drivers (e.g., core hub modification, high cognitive complexity, single-owner risk).
* Produces an automated, explainable markdown review summary suitable for GitHub PR comments.

### 6. 🧭 Contributor Guidance Matrix
Classifies files into actionable developer zones:
* 🟢 **`BEGINNER_FRIENDLY`**: Low complexity, isolated coupling, minimal downstream dependencies.
* 🔴 **`HIGH_RISK_CORE`**: High complexity combined with massive architectural coupling.
* 🟡 **`SINGLE_MAINTAINER_SILO`**: Over 80% written by a single author.
* 🔵 **`HIGH_COUPLING`**: High efferent coupling importing dozens of external modules.

---

## 🏗️ System Architecture

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                   INPUT LAYER                                          │
│           Local Codebase Path (.)   OR   Remote Git Repository URL (HTTPS/SSH)          │
└───────────────────────────────────────────┬────────────────────────────────────────────┘
                                            │
                                            ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              INGESTION & EXTRACTION                                    │
│  ┌─────────────────────────┐   ┌──────────────────────────┐   ┌─────────────────────┐  │
│  │   Git Cloner & Cache    │   │   Streaming Git Miner    │   │ Polyglot AST Engine │  │
│  │  (Workspace Isolation)  │   │  (PyDriller + Git CLI)   │   │  (Python/JS/TS/Go)  │  │
│  └────────────┬────────────┘   └────────────┬─────────────┘   └──────────┬──────────┘  │
└───────────────┼─────────────────────────────┼────────────────────────────┼─────────────┘
                │                             │                            │
                ▼                             ▼                            ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                   CORE ENGINES                                         │
│  ┌─────────────────────────┐   ┌──────────────────────────┐   ┌─────────────────────┐  │
│  │  Dependency Graph Engine│   │       Health Engine      │   │  ML Dataset Builder │  │
│  │   (NetworkX DiGraph)    │   │   • Tornhill Hotspots    │   │  • Temporal Split   │  │
│  │   • PageRank            │   │   • Bus Factor / Gini    │   │  • Leak-free X ∈ R18│  │
│  │   • Martin Instability  │   │   • Cyclomatic Score     │   │  • Forward Labels Y │  │
│  │   • Cycle Detection     │   │   • Churn Aggregation    │   │                     │  │
│  └────────────┬────────────┘   └────────────┬─────────────┘   └──────────┬──────────┘  │
└───────────────┼─────────────────────────────┼────────────────────────────┼─────────────┘
                │                             │                            │
                ▼                             ▼                            ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          GRAPH NEURAL NETWORK & PREDICTION                             │
│  ┌──────────────────────────────────────────────┐   ┌──────────────────────────────┐   │
│  │      PyTorch Geometric GraphSAGE (GNN)       │   │    Pull Request Simulator    │   │
│  │      • Inductive Neighbor Aggregation        │   │    • Reverse-BFS Traversal   │   │
│  │      • Multi-Task Risk & Impact Heads        │   │    • Topological Proof       │   │
│  └──────────────────────┬───────────────────────┘   └──────────────┬───────────────┘   │
└─────────────────────────┼──────────────────────────────────────────┼───────────────────┘
                          │                                          │
                          ▼                                          ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                     API LAYER                                          │
│               FastAPI Asynchronous REST Service (/api/v1/analyze, /api/v1/predict/pr)   │
└───────────────────────────────────────────┬────────────────────────────────────────────┘
                                            │
                                            ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                 PRESENTATION LAYER                                     │
│            React 18 + TypeScript + Vite + Tailwind CSS + Cytoscape.js Dashboard        │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📐 Mathematical Foundations

### 1. McCabe Cyclomatic Complexity
Measures the number of linearly independent paths through program modules:
$$M = E - N + 2P$$
Where $E$ is the number of edges in the control flow graph, $N$ is the number of nodes, and $P$ is the number of connected components.

### 2. Robert C. Martin's Package Instability
Evaluates resilience to architectural changes based on coupling:
$$I = \frac{C_e}{C_a + C_e} \quad \in [0, 1]$$
* **Afferent Coupling ($C_a$)**: Inbound dependencies (modules depending on this file).
* **Efferent Coupling ($C_e$)**: Outbound dependencies (modules this file depends on).
* $I = 0 \implies$ Maximally stable (rigid core foundation).
* $I = 1 \implies$ Maximally unstable (volatile top-level layer).

### 3. Team Contribution Gini Coefficient
Quantifies knowledge centralization across developers $k \in \{1, \dots, n\}$ with commit counts $y_1 \le y_2 \le \dots \le y_n$:
$$G = \frac{\sum_{i=1}^n \sum_{j=1}^n |y_i - y_j|}{2n \sum_{i=1}^n y_i}$$
* $G \to 0$: Democratic, evenly distributed codebase familiarity.
* $G \to 1$: Extreme concentration of ownership in a single developer (high Bus Factor risk).

### 4. GraphSAGE Message-Passing
Computes inductive file embeddings $\mathbf{h}_v^{(k)}$ across neighborhood $\mathcal{N}(v)$:
$$\mathbf{h}_{\mathcal{N}(v)}^{(k)} = \text{AGGREGATE}_k \left( \left\{ \mathbf{h}_u^{(k-1)}, \forall u \in \mathcal{N}(v) \right\} \right)$$
$$\mathbf{h}_v^{(k)} = \sigma \left( \mathbf{W}^{(k)} \cdot \left[ \mathbf{h}_v^{(k-1)} \,\|\, \mathbf{h}_{\mathcal{N}(v)}^{(k)} \right] \right)$$

### 5. Downstream Blast Radius
Given a pull request modifying files $P \subset \mathcal{V}$, the total downstream blast radius $\mathcal{B}(P)$ is computed via reverse transitive closure:
$$\mathcal{B}(P) = \bigcup_{u \in P} \left\{ v \in \mathcal{V} \mid \text{path}(v \rightsquigarrow u) \text{ exists in } \mathcal{G} \right\}$$

---

## 📁 Repository Structure

```text
repoinsight/
├── pyproject.toml              # Python package configuration & dependencies
├── uv.lock                     # Reproducible dependency lockfile
├── .gitignore                  # Git exclusion rules
├── README.md                   # Project overview & documentation
├── PROJECT_EXPLAINER.md        # In-depth architectural & conceptual guide
│
├── src/repoinsight/            # Python backend core package
│   ├── ingestion/              # Repository target validation & idempotent cloner
│   │   ├── validator.py
│   │   └── cloner.py
│   ├── mining/                 # Git history mining & streaming commit extraction
│   │   ├── models.py
│   │   └── miner.py
│   ├── analysis/               # Multi-language AST parsing & complexity scoring
│   │   ├── models.py
│   │   ├── python_parser.py
│   │   ├── polyglot.py
│   │   └── scanner.py
│   ├── graph/                  # NetworkX dependency graph construction & metrics
│   │   ├── builder.py
│   │   ├── metrics.py
│   │   └── exporter.py
│   ├── health/                 # Health engine, Tornhill hotspots & Bus Factor
│   │   ├── churn.py
│   │   ├── hotspots.py
│   │   ├── ownership.py
│   │   └── engine.py
│   ├── guidance/               # Contributor difficulty & onboarding classification
│   │   └── recommender.py
│   ├── ml/                     # ML dataset generation, baseline models & GNN
│   │   ├── labels.py
│   │   ├── dataset.py
│   │   ├── baseline.py
│   │   ├── gnn.py
│   │   ├── predictor.py
│   │   └── explainer.py
│   └── api/                    # FastAPI web server & Pydantic v2 schemas
│       ├── schemas.py
│       └── main.py
│
└── frontend/                   # React + TypeScript + Vite Dashboard
    ├── package.json
    ├── vite.config.ts
    ├── tailwind.config.js
    └── src/
        ├── App.tsx             # Main dashboard layout & orchestration
        ├── types.ts            # Frontend TypeScript data interfaces
        └── components/
            ├── Header.tsx                  # Navbar & repository search form
            ├── HealthOverview.tsx          # Health score card & sub-score pillars
            ├── DependencyGraphView.tsx     # Interactive Cytoscape.js graph canvas
            ├── PullRequestSimulator.tsx    # Live PR blast radius calculator
            └── ContributorGuidanceView.tsx # Contributor difficulty cards
```

---

## 🚀 Getting Started

### Prerequisites
* **Python 3.10+** (Python 3.10 or 3.11 recommended)
* **Node.js 18+** & **npm**
* **Git** installed and available on system `PATH`

### 1. Clone the Repository
```bash
git clone https://github.com/Sanshwit05/RepoInsight.git
cd RepoInsight
```

### 2. Set Up the Backend
We recommend using a Python virtual environment:

```bash
# Create and activate virtual environment
python -m venv .venv

# On Linux / macOS:
source .venv/bin/activate

# On Windows (PowerShell / Git Bash):
.venv\Scripts\activate

# Install dependencies (using pip or uv)
pip install -e .
```

Start the FastAPI server:
```bash
uvicorn src.repoinsight.api.main:app --host 127.0.0.1 --port 8000 --reload
```
* The API will be accessible at: `http://127.0.0.1:8000`
* Interactive Swagger UI docs: `http://127.0.0.1:8000/docs`

### 3. Set Up the Frontend
Open a separate terminal window:

```bash
cd frontend

# Install Node dependencies
npm install

# Start Vite development server
npm run dev
```
* The frontend web app will run at: `http://localhost:5173`

### 4. Running an Analysis
1. Navigate to `http://localhost:5173` in your browser.
2. In the target input bar, enter:
   * `.` to analyze the local RepoInsight codebase itself, OR
   * A local path (e.g., `D:/my-project`), OR
   * Any public GitHub repository URL (e.g., `https://github.com/pallets/flask`).
3. Click **Analyze Repository** to view real-time maintainability metrics, hotspots, dependency graphs, and test the PR simulator.

---

## 📡 API Reference

### `POST /api/v1/analyze`
Executes an end-to-end audit of a local directory or remote Git URL.

**Request Body**:
```json
{
  "target": ".",
  "max_commits": 100,
  "include_external": false
}
```

**Response Snapshot**:
```json
{
  "repository": "repoinsight",
  "health_score": 88.4,
  "sub_scores": {
    "complexity_score": 85.2,
    "churn_score": 91.0,
    "ownership_score": 87.5,
    "coupling_score": 90.0
  },
  "bus_factor": 2,
  "hotspots": [
    {
      "file_path": "src/repoinsight/health/engine.py",
      "cyclomatic_complexity": 8.4,
      "churn_score": 0.82,
      "is_hotspot": true
    }
  ],
  "graph": {
    "total_nodes": 24,
    "total_edges": 38,
    "elements": [...]
  },
  "contributor_guidance": [...]
}
```

### `POST /api/v1/predict/pr`
Simulates the downstream impact and blast radius of modifying specific files in a Pull Request.

**Request Body**:
```json
{
  "target": ".",
  "touched_files": [
    "src/repoinsight/graph/builder.py"
  ]
}
```

**Response Snapshot**:
```json
{
  "risk_level": "MEDIUM",
  "predicted_impact_score": 5.4,
  "blast_radius_files": [
    "src/repoinsight/health/engine.py",
    "src/repoinsight/api/main.py"
  ],
  "key_risk_drivers": [
    "High afferent coupling: 2 upstream modules depend on modified files."
  ],
  "markdown_summary": "### 🔍 RepoInsight PR Impact Report\n..."
}
```

---

## 🗺️ Project Roadmap

- [x] **Phase 0–2**: Repository Ingestion, URL Parsing & Idempotent Cloner
- [x] **Phase 3**: Streaming Git Commit Miner (PyDriller + Git CLI fallback)
- [x] **Phase 4**: Polyglot Static Analysis (Python AST McCabe, TS/JS/Go regex scanner)
- [x] **Phase 5**: Directed Import Dependency Graph & Topological Metrics (NetworkX)
- [x] **Phase 6**: Health Engine, Tornhill Hotspots & Avelino Bus Factor
- [x] **Phase 7**: Contributor Guidance Matrix & Difficulty Classification
- [x] **Phase 8**: Leak-Free Chronological ML Dataset Generation ($\mathbf{X} \in \mathbb{R}^{18}$)
- [x] **Phase 9**: Baseline Machine Learning (Random Forest & Gradient Boosting)
- [x] **Phase 10**: Deep Graph Neural Network (PyTorch GraphSAGE)
- [x] **Phase 11**: Pull Request Impact Predictor & Reverse-BFS Blast Radius
- [x] **Phase 12**: Explainability Engine & GitHub PR Comment Formatter
- [x] **Phase 13**: FastAPI Backend REST Service with Pydantic v2 Contracts
- [x] **Phase 14**: React 18 + Vite + Tailwind CSS + Cytoscape.js Dashboard
- [ ] **Phase 15**: Persistence Layer (SQLite / SQLAlchemy Audit History & PR Logs)
- [ ] **Phase 16**: Automated Unit & Integration Test Suite (Pytest)
- [ ] **Phase 17**: Docker Containerization & GitHub Actions CI/CD Pipeline

---

## ⚖️ License

This project is licensed under the [MIT License](LICENSE) — see the LICENSE file for details.

---

<div align="center">
  <sub>Developed with ❤️ for software engineering teams and open-source communities.</sub>
</div>
