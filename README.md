

---

# 🧠 RAG System - Health & Wellness Assistant

## 🩺 Topic: Health, Fitness, and Wellbeing

I chose **health, fitness, and wellbeing** as my topic because it provides practical, structured guidance suitable for text analysis and allows testing the LLM’s ability to handle real-world instructional content with clear, actionable information.

---

## 🔧 What I Changed

### 🗂️ Documents

Replaced the dataset with **5 health-focused PDFs**:

* `10-Tips-Healthy-Lifestyle.pdf` — Practical lifestyle recommendations
* `Adult-Guide-to-an-Active-Healthy-Lifestyle.pdf` — Comprehensive activity guidelines
* `how-can-i-make-lifestyle-healthier.pdf` — Lifestyle improvement strategies
* `nnm_tipsheet.pdf` — Nutrition and meal planning tips
* `PAG_ExecutiveSummary.pdf` — Physical activity guidelines summary

---

### ✂️ Chunking

**Modified chunking configuration:**

```python
# Tested two configurations:
configs = [
    {"name": "Small chunks", "chunk_size": 400, "chunk_overlap": 200},
    {"name": "Large chunks", "chunk_size": 1200, "chunk_overlap": 50}
]

# Selected large chunks for better performance
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1200,
    chunk_overlap=50,
    separators=["\n\n", "\n", ". ", "! ", "? ", " ", ""]
)
```

**Which settings worked better:**
Large chunks (1200/50) worked significantly better because they produced **58 well-structured chunks** with sufficient context and minimal redundancy, compared to **234 fragmented chunks** from small settings.
The larger chunks maintained better readability and provided more effective context for the LLM to process health-related content.

---

### 🔍 Retrieval

**Improved retriever configuration:**

```python
retriever = vectorstore.as_retriever(
    search_type="mmr",       # Use Maximum Marginal Relevance
    search_kwargs={
        "k": 3,              # Return top 3 chunks per query
        "fetch_k": 10,       # Consider top 10 candidates before MMR
        "lambda_mult": 0.5   # Balance between relevance and diversity
    }
)
```

**Test questions demonstrated:**

1. “How much sleep do adults need?” → Retrieved “7–9 hours of sleep daily.”
2. “How much physical activity should adults get per week?” → Retrieved “150 minutes moderate or 75 minutes vigorous activity per week.”

---

## ⚙️ How to Run

### Prerequisites

1. Install **Ollama** from [https://ollama.ai/](https://ollama.ai/)
2. Pull the required model:

   ```bash
   ollama pull phi3:mini
   ```
3. Verify installation:

   ```bash
   ollama list
   ```

---

### Installation

```bash
pip install langchain==0.3.27
pip install langchain-community==0.3.29
pip install chromadb==1.0.20
pip install pypdf==6.0.0
pip install numpy==6.0.0
```

---

### Execution

1. Ensure all 5 PDF documents are in the `./data/` folder.
2. Open and run the Jupyter notebook: `Nandana.M_workshop_hc_assignment.ipynb`.
3. Execute cells sequentially from top to bottom.
4. The system will automatically process documents and be ready for queries.

---

## 🧩 Test Questions

1. “How much sleep do adults need?”
2. “How much physical activity should adults get per week?”

---

## 🌟 System Features

* **Document Processing:** Automatically loads and processes 5 health PDF documents
* **Optimized Chunking:** Large chunk sizes (1200/50) for better context preservation
* **Enhanced Retrieval:** MMR search with balanced relevance/diversity (`k=3`, `fetch_k=10`, `lambda_mult=0.5`)
* **Local LLM Integration:** Uses `phi3:mini` model via Ollama for private, offline processing
* **Quality Validation:** Built-in answer validation to detect potential hallucinations
* **Source Citation:** Tracks and cites original document sources for verification
* **Health Domain Specialization:** Tuned specifically for health and wellness content

---

## 📁 Project Structure

```
├── data/                          # 5 Health PDF documents
│   ├── 10-Tips-Healthy-Lifestyle.pdf
│   ├── Adult-Guide-to-an-Active-Healthy-Lifestyle.pdf
│   ├── how-can-i-make-lifestyle-healthier.pdf
│   ├── nnm_tipsheet.pdf
│   └── PAG_ExecutiveSummary.pdf
├── Nandana.M_workshop_hc_assignment.ipynb  # Main workshop notebook
└── README.md                      # This file
```

---

## 🧾 Results

The system successfully demonstrates improved performance with:

* Better chunking strategy for health content
* Optimized retrieval parameters for relevant document fetching
* Accurate responses to health-related queries with proper source citation
* Effective grounding in the provided health documents

This RAG system is specifically tuned for **health and wellness content**, providing reliable, document-grounded answers to fitness and lifestyle questions.

---


