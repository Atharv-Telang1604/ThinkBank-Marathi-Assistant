# ThinkBank Marathi Assistant

A Colab notebook that builds an interactive Marathi-language question-answering chatbot over a collection of local PDF transcripts. The notebook extracts Marathi text from PDFs, cleans and splits it into chunks, converts chunks to embeddings, stores them in a FAISS vector store, and uses a Gemini generative model to answer user queries with source citations via a Gradio chat UI.

Notebook
- Gemini2_Think_Bank (3).ipynb — Google Colab notebook (path: `Gemini2_Think_Bank (3).ipynb`).

Key features
- Upload Marathi PDF transcripts into `/content/pdfs` (Colab session folder).
- Extract selectable text from PDFs using pdfplumber.
- Clean Marathi text and split into chunks (RecursiveCharacterTextSplitter).
- Create multilingual embeddings with sentence-transformers and store them in FAISS.
- Perform vector similarity search to retrieve context for queries.
- Use a Gemini generative model to produce Marathi answers constrained to the retrieved context.
- Launch an interactive Gradio chat interface that returns the answer and its source PDF(s).

Quick start (recommended: run in Google Colab)
1. Open the notebook in Colab: https://github.com/Atharv-Telang1604/ThinkBank-Marathi-Assistant/blob/main/Gemini2_Think_Bank%20(3).ipynb
2. Run the first cell and upload your Marathi PDF files when prompted — uploaded files are saved to `/content/pdfs`.
3. Install dependencies (the notebook includes a cell to install them):

```bash
!pip install -q -U google-genai pdfplumber gradio langchain-text-splitters langchain-community faiss-cpu sentence-transformers google-generativeai
```

4. Set credentials in the notebook:
- Replace the placeholder value of `MY_API_KEY` with your Google Generative AI (Gemini) API key.
- (Optional) Set a `HF_TOKEN` in Colab secrets for authenticated Hugging Face downloads and higher rate limits.

5. Run the cells in order, or use the Gradio UI:
- Click the "🔄 डेटा लोड / रिफ्रेश करा (Refresh Dataset)" button to ingest PDFs and build the FAISS vector store.
- Use the Gradio chat to ask questions in Marathi. Each answer will include the source PDF(s) used.

Configuration
- MODEL_NAME — Gemini model used (default in the notebook: `gemini-2.5-flash-lite`).
- DATASET_FOLDER — path for input PDFs (default: `/content/pdfs`).
- Embedding model — currently `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`.

Architecture overview
1. PDF upload → saved to /content/pdfs.
2. Text extraction with pdfplumber → cleaned by `clean_marathi_text()`.
3. Chunking with RecursiveCharacterTextSplitter.
4. Embeddings created with a sentence-transformers model.
5. FAISS vector store built from embeddings.
6. On query: retrieve top-K similar chunks → construct context → call Gemini to generate answer constrained to that context → return answer + source list.

Important notes & recommendations
- google.generativeai deprecation: The notebook uses the `google.generativeai` / `google-genai` packages. The `google.generativeai` package may be deprecated in some environments; consider migrating to `google.genai` if you update the libraries or encounter warnings.
- Hugging Face authentication: Without a `HF_TOKEN`, downloads and requests are unauthenticated and may be rate-limited. Set `HF_TOKEN` in Colab secrets if you process many files or need faster downloads.
- Scanned PDFs / OCR: pdfplumber extracts selectable text. If your PDFs are scanned images, add an OCR step (e.g., Tesseract) before ingestion.
- Persistent storage: FAISS in the notebook is in-memory. If you need persistence between sessions, save the vector store to disk or to a cloud bucket and add load/save code.
- Gradio sharing: When running in Colab, Gradio may expose a temporary public link (expires in ~1 week). Do not upload sensitive documents if you enable sharing.

Troubleshooting
- Empty results / no answer: confirm uploaded PDFs contain extractable text (not only scanned images), and that `ingest_directory()` completed successfully.
- HF model warnings / failures: set `HF_TOKEN` or try re-running the cell if model downloads fail.
- Gemini API errors (e.g. 503): verify your `MY_API_KEY`, check quota/region availability, and retry after a short delay.

Security & privacy
- Uploaded files are stored only in the Colab VM under `/content` for the active session unless you explicitly save them elsewhere.
- If you publish or share the Gradio link, anyone with the link can access the running UI and its data while the session runs. Remove sensitive files or run locally behind private networking for sensitive data.

Suggested next improvements
- Add OCR (Tesseract or Google Vision) to support scanned PDFs.
- Persist the FAISS index to disk or cloud to avoid rebuilding every session.
- Replace deprecated package usage (`google.generativeai`) with `google.genai` and update the generation calls as needed.
- Optionally switch to a hosted embeddings service or a managed vector DB for scale.

License
- No license is included by default. Add a LICENSE file (MIT, Apache-2.0, etc.) if you plan to publish or share this repository.

Contact / Support
- If you want, I can:
  - Translate this README to Marathi.
  - Add a LICENSE (choose MIT, Apache-2.0, GPL-3.0, or tell me which license you prefer).
  - Generate a requirements.txt / environment.yml.
  - Add example code to persist the FAISS index or add OCR steps.

---
README updated to be clearer and more professional. If you'd like wording changes, additional sections, or Marathi translation, tell me which and I'll update the file.