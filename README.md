# ThinkBank Marathi Assistant

This repository contains a Google Colab Jupyter notebook that builds a Marathi-language document question-answering chatbot using local PDF transcripts and a combination of embeddings + a Gemini generative model.

Notebook
- Gemini2_Think_Bank (3).ipynb — main Colab notebook. It:
  - uploads PDF files into /content/pdfs
  - extracts Marathi text from PDFs with pdfplumber
  - cleans and chunks text (RecursiveCharacterTextSplitter)
  - creates embeddings using sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2
  - builds a FAISS vector store
  - queries the vector store for relevant context and calls a Gemini model to generate Marathi answers
  - launches a Gradio chat UI for interactive queries

Quick start (recommended: run in Google Colab)
1. Open the notebook: Gemini2_Think_Bank (3).ipynb
2. Upload your Marathi PDF files by running the first cell (it saves files to /content/pdfs).
3. Install dependencies (the notebook includes a pip install cell). Example:

   !pip install -q -U google-genai pdfplumber gradio langchain-text-splitters langchain-community faiss-cpu sentence-transformers google-generativeai

4. Set credentials:
   - Replace MY_API_KEY in the notebook with your Google Generative AI (Gemini) API key.
   - (Optional) For faster Hugging Face model downloads and higher rate limits, add a `HF_TOKEN` in Colab secrets.

5. Run the cells in order, or press the Gradio "🔄 डेटा लोड / रिफ्रेश करा (Refresh Dataset)" button in the UI to ingest PDFs and construct the vector DB.
6. Ask questions in Marathi using the Gradio chat interface. The app returns an answer and lists the source PDF(s) used.

Important notes
- The notebook currently uses the `gemini-2.5-flash-lite` model via the `google.generativeai` / `google-genai` libraries. You must provide a valid Gemini API key in the `MY_API_KEY` variable before generating responses.
- The notebook prints warnings about `google.generativeai` being deprecated. Consider switching to `google.genai` if you update packages and APIs.
- The notebook uses the sentence-transformers model `paraphrase-multilingual-MiniLM-L12-v2` for embeddings; the notebook warns about unauthenticated HF usage. If you plan to process many or large files, set HF_TOKEN.
- Gradio will expose a temporary share URL when running in Colab (expires in one week). For permanent hosting, deploy to Hugging Face Spaces or another hosting platform.

Files saved by the notebook
- /content/pdfs — where uploaded PDFs are stored during a Colab session.

Configuration options you may want to change
- MODEL_NAME — the Gemini model to call (currently `gemini-2.5-flash-lite`).
- MY_API_KEY — set to your Gemini API key.
- DATASET_FOLDER — path for PDFs (default `/content/pdfs`).
- Embedding model — currently a sentence-transformers multilingual MiniLM model.

Troubleshooting
- If you see rate-limit or authentication warnings from Hugging Face, set HF_TOKEN in Colab secrets.
- If generation calls fail (e.g., 503), check network connectivity and that your Gemini API key is correct and has usage/quota.
- If no answers are found, ensure PDFs contain extractable selectable text (pdfplumber may not extract text reliably from scanned images without OCR).

Privacy and data
- Uploaded PDFs are saved only to the Colab VM (under /content). If you share a Gradio public link the data could be accessible to anyone with the link while the session is running. Do not upload sensitive documents without ensuring appropriate controls.

License
- Add a license if you want to publish this project. (No license is included by default.)

Contact / next steps
- To improve results, consider adding OCR (Tesseract) for scanned PDFs, choosing a higher-capacity embedding model, or storing the vector DB persistently instead of only in-memory FAISS.

---
*Generated README for the uploaded notebook by GitHub Copilot.*
