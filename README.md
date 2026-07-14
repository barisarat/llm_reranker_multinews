# Diagnosing LLM Reranker Behavior Under Fixed Evidence Pools

> **Diagnosing LLM Reranker Behavior Under Fixed Evidence Pools**
> Baris Arat and Emre Sefer - SIGIR '26, Melbourne, Australia
> DOI: [10.1145/3805712.3809852](https://doi.org/10.1145/3805712.3809852)

---

## Repository Structure

```
.
├── 1-multinews-exp.ipynb          # Run Multi-News fixed-pool experiments
├── 2-0-multinews-results.ipynb    # Reconstruct Multi-News result tables
├── 2-1-viz_frontier.ipynb         # visualizations
├── 2-2-viz_redundancy.ipynb       # visualizations
├── 3-trec-data-prep.ipynb         # Prepare TREC-DL 2021-2023 dataset
├── 4-trec-results.ipynb           # Reconstruct TREC-DL result tables
├── colab_notebooks/               # Colab model run notebooks (CE, GPT, LLAMA, QWEN)
├── colab_exports/                 # Prepared TREC-DL data written by notebook 3
├── colab_imports/                 # Model run outputs from Colab, read by notebook 4
├── runs_fixed_pool_rerank/        # Saved Multi-News run artifacts
├── msmarco_v2_passage.tar         # Required for TREC-DL data prep
├── semantic_eval_results.json
└── .env                           
```

---

## Setup

**Python 3.10+** (tested on 3.14.4)

```bash
pip install datasets ir-datasets rank-bm25 sentence-transformers numpy pandas requests python-dotenv matplotlib scikit-learn

# For local model notebooks only:
pip install transformers torch bitsandbytes
```

**Local models (Multi-News Llama/Qwen runs):** install [Ollama](https://ollama.com), then:

```bash
ollama pull llama3.1:8b-instruct-q4_K_M
ollama pull qwen2.5:7b-instruct-q4_K_M
```

Ollama is assumed to be accessible at `http://localhost:11434`.

**GPT runs:** create a `.env` file at the project root:

```
OPENAI_API_KEY=your_key_here
```

---

## Reproducing Paper Results

### Option A - From saved results (no model inference needed)

Uses existing `runs_fixed_pool_rerank/` for Multi-News and `colab_imports/` for TREC-DL. To reproduce the reported results from saved files, run only the reporting notebooks:

```
2-1-viz_frontier.ipynb        (optional figures)
2-2-viz_redundancy.ipynb      (optional figures)
4-trec-results.ipynb
```

No Ollama service or OpenAI API key is required for this step.

### Option B - Full rerun

**Multi-News** (local GPU sufficient):

1. `1-multinews-exp.ipynb` - runs experiments, writes artifacts to `runs_fixed_pool_rerank/`
2. `2-0-multinews-results.ipynb` - builds result tables

**TREC-DL** (data prep and reporting local, model runs in Colab):

1. `3-trec-data-prep.ipynb` - requires [msmarco_v2_passage.tar](https://ir-datasets.com/msmarco-passage-v2.html) present locally, writes to `colab_exports/`
2. Run the following notebooks in Colab:
- `colab_notebooks/CE.ipynb`
- `colab_notebooks/GPT.ipynb`
- `colab_notebooks/LLAMA.ipynb`
- `colab_notebooks/QWEN.ipynb`
3. Copy the resulting model output folders into `colab_imports/` 
4. `4-trec-results.ipynb` - rebuilds baselines (BM25, MMR, Random) and final tables

---

## Contact

For questions about the code or experiments, contact the authors at the emails listed in the paper.
