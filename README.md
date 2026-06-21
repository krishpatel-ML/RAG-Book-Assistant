# 📚 RAG Book Assistant

A Retrieval-Augmented Generation (RAG) app that lets you upload any PDF and ask questions about its content. The app retrieves relevant chunks from the document and uses an LLM to answer based only on that context — reducing hallucination and keeping answers grounded in the source material.

## Features

- 📄 Upload any PDF book/document via a simple Streamlit interface
- 🔍 Automatic text chunking and embedding generation
- 🧠 Semantic search using MMR (Maximal Marginal Relevance) retrieval
- 💬 Ask natural language questions and get context-grounded answers
- 🚫 Explicitly states when an answer isn't found in the document (no hallucinated answers)

## Tech Stack

- **[Streamlit](https://streamlit.io/)** — Web UI
- **[LangChain](https://www.langchain.com/)** — RAG orchestration (document loading, splitting, prompting)
- **[ChromaDB](https://www.trychroma.com/)** — Vector store for document embeddings
- **[Mistral AI](https://mistral.ai/)** — Embeddings (`mistral-embed`) and chat completion (`mistral-small`)

## How It Works

1. **Upload** — User uploads a PDF through the Streamlit interface.
2. **Load & Split** — The PDF is loaded with `PyPDFLoader` and split into overlapping chunks (1000 chars, 200 overlap) using `RecursiveCharacterTextSplitter`.
3. **Embed & Store** — Each chunk is embedded using Mistral's `mistral-embed` model and stored in a local ChromaDB vector store.
4. **Retrieve** — When a question is asked, the retriever fetches the most relevant chunks using MMR search (balances relevance and diversity).
5. **Generate** — The retrieved chunks are passed as context to `mistral-small` via a prompt template, which answers strictly from that context.

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/<your-username>/RAG-Book-Assistant.git
cd RAG-Book-Assistant
```

### 2. Create a virtual environment
```bash
python -m venv .venv
.venv\Scripts\activate      # Windows
source .venv/bin/activate    # macOS/Linux
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Set up environment variables
Copy `.env.example` to `.env` and add your Mistral API key:
```
MISTRAL_API_KEY=your_mistral_api_key_here
```

### 5. Run the app
```bash
streamlit run app.py
```

## Usage

1. Upload a PDF using the file uploader.
2. Click **"Create Vector Database"** to process and embed the document.
3. Type your question in the text box and get an answer grounded in the PDF's content.

## Known Limitations

- Uploading a new PDF currently adds to the existing vector store rather than replacing it, so questions may retrieve context from previously uploaded PDFs in the same session. A fix (clearing/resetting the vector store per upload) is planned.
- Large PDFs will take longer to embed due to API rate limits on the embedding calls.

## Future Improvements

- [ ] Reset vector store per new PDF upload
- [ ] Support multiple PDF uploads with source attribution
- [ ] Add chat history / conversational memory
- [ ] Deploy on Streamlit Community Cloud

## License

This project is open source and available under the [MIT License](LICENSE).
