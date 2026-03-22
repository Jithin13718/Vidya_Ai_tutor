# 📚 Vidya: AI Instructor for India's Rural Areas

> AI-powered schooling is feasible over 2G networks thanks to an intelligent teaching system that uses **Context Pruning** to cut LLM token usage by 85–92%.

---

## 🌿 What is Vidya?

Vidya is a two-part project:
| File | What it is | 
|------|-----------| 
| `index.html` | A single-file web application that allows you to upload any PDF text---
|'context_purning_rag.ipynb'| A google colab will implement the this file that is ragpipeline with cost comparison|
---
## The Fundamental Method: Context Pruning

For each question, Standard RAG gives the full book as context. **Context Pruning** reduces tokens by 85–92% by first identifying pertinent chapters and then sending only those.
Each chapter is scored by keyword match (no API call) based on the student question and context pruning.
Only one or two of the thirteen chapters are relevant. Groq LLM obtains about 3,000 tokens instead of over 40,000. The answer comes from your actual textbook.

---

## 🚀 How to Use

### Web App (`education_tutor.html`)
1. Visit [console.groq.com](https://console.groq.com) to obtain a free Groq API key. 2. Launch `index.html` in any contemporary browser (Chrome, Firefox, Edge).

3. Click **Save** after entering your Groq API key.
4. Upload your textbook in PDF format.
5. Give OCR a minute or two to extract text from every chapter (one-time setup).
6. After setup, you can ask any question and receive immediate responses! Because text is extracted using Tesseract.js OCR rather than native PDF text parsing, it works with any PDF, including those with faulty font encoding (such as NCERT textbooks).

> **Works with any PDF** — including PDFs with broken font encoding (like NCERT textbooks), because text is extracted using Tesseract.js OCR, not native PDF text parsing.

### Colab Notebook (`context_pruning_rag.ipynb`)

1. Launch [Google Colab] (https://colab.research.google.com).
2. Execute every cell sequentially
3. When asked, upload your PDF.
4. On test questions, the notebook uses **both** the baseline RAG and Context Pruning methods.
5. The token and cost savings are displayed in a cost summary table.
---

## ⚙️ Technologies Used

### Web App

| Technology | Goal | 
|-----------|---------| 
| **PDF.js** (Mozilla) | Renders PDF documents to Canvas in the browser | 
| **Tesseract.js v5** | Browser-based OCR engine (WebAssembly) — reads pages visually, avoids broken fonts | 
| **Groq API** + `llama-3.1-8b-instant` | Quick, inexpensive LLM for producing replies from text | 
| Plain HTML / CSS / JS | No frameworks, no build tools, no server required | 

### Colab Notebook 
| Technology | Goal | 
|-----------|---------| 
| **LangChain** | RAG pipeline framework | 
| **pdfplumber** | PDF text extraction | 
| **FAISS** | Vector store for baseline RAG | 
| **HuggingFace Embeddings** | `sentence-transformers/all-MiniLM-L6-v2` | 
| **LangChain-Groq** | Groq LLM integration |
---

## 📊 Cost Comparison Metric 

| Baseline RAG | Context Pruning | 

|--------|-------------|----------------| 

| Tokens per query | ~40,000 | ~3,000 | 

| Data per query | ~160 KB | ~12 KB | 

| Cost per query | ~0.0018 | ~0.00017 | 

| **Token reduction** | — | **85–92%** |

---

## 📁 Repository ConfigurationThe entire online application (one file, open in browser) may be found at vidya/
-> education_tutor.html

-> context_pruning_rag.ipynb Google Colab laptop with price comparison

->README.md ```

->requirements.txt

---

## 🌐 Why 2G-Friendly?

It takes about one to two minutes to set up OCR (preferably over WiFi).
After setup, each query merely transmits about 4–6 KB of plain text to Groq.
At query time, no images are sent.
4-6 KB can be handled in less than a second with a 2G connection (50–100 kbps).
---

## 🔑 Obtaining a Groq API Key

1. Visit [console.groq.com](https://console.groq.com)
2. Create a free account
3. Select **API Keys** → **Create API Key**.
4. Make a copy of the key (beginning with `gsk_...`).
5. Paste it into the notepad or the web application.

For hundreds of searches, Groq's free tier is adequate. Paste it into the web app or the notebook

---

## 📖 How Chapters Are Scored by Context Pruning
python 
for every chapter:
    score = 0
    for every keyword in question (words longer than three characters):
        if keyword in chapter_title: score += 3 #strong signal 
        if keyword in chapter_text: score += 1 #weaker signal

Choose the top two chapters based on score, then discard the rest.
This is fully implemented in Python (notebook) or JavaScript (web application); no further API calls are required for trimming.

---

## ⚠️ Limitations

- OCR samples 3 pages per chapter (first, middle, last). Content on other pages may be missed.
- Chapter detection uses regex for `"Chapter N"` headings. Non-standard formats fall back to 20-page equal splits.
- The Groq API key is stored in browser memory only — not persisted between sessions.
- Refreshing the page requires re-running OCR.

---

## 📄 License

MIT — free to use, modify, and distribute.
