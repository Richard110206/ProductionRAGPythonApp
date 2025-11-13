## ProductionRAGPythonApp
![Python](https://img.shields.io/badge/Python-%3E%3D3.13-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-%3E%3D0.116.1-green.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-%3E%3D1.49.1-red.svg)
![Uvicorn](https://img.shields.io/badge/Uvicorn-%3E%3D0.35.0-purple.svg)
![inngest](https://img.shields.io/badge/inngest-%3E%3D0.5.6-orange.svg)
![llama-index-core](https://img.shields.io/badge/llama--index--core-%3E%3D0.14.0-yellow.svg)
![openai](https://img.shields.io/badge/openai-%3E%3D1.107.0-blue.svg)
![qdrant-client](https://img.shields.io/badge/qdrant--client-%3E%3D1.15.1-indigo.svg)
![python-dotenv](https://img.shields.io/badge/python--dotenv-%3E%3D1.1.1-lightgrey.svg)

A production-ready RAG (Retrieval-Augmented Generation) application that allows users to upload PDF files and query information from them using AI. It is specifically optimized for users in mainland China who may face difficulties accessing OpenAI API keys.For those who can use OpenAI API keys, you may refer to this repository:[ProductionGradeRAGPythonApp](https://github.com/techwithtim/ProductionGradeRAGPythonApp).And specific details on how to run the program are available on the YouTube channel in this video:[How to Build a Production-Ready RAG AI Agent in Python (Step-by-Step)](https://www.youtube.com/watch?v=AUQJ9eeP-Ls&t=4425s)

### Features

- PDF upload and automatic text chunking
- Vector storage using Qdrant
- Embedding generation using BAAI/bge-m3 model via SiliconFlow
- AI-powered query answering using DeepSeek
- FastAPI backend with Inngest workflow management
- Streamlit frontend for user interaction

### Requirements

- Python 3.10 or higher
- Qdrant (local or cloud instance)
- Inngest server
- Node.js (for running Inngest CLI)
- uv (Python package installer, optional but recommended)

### Environment Setup

1. Create a `.env` file in the root directory with the following variables:
``` plaintext
SILICONFLOW_API_KEY=your_siliconflow_api_key   # or API for other text embedding models
DEEPSEEK_API_KEY=your_deepseek_api_key   # or any AI capable of conversational interactions
INNGEST_API_BASE=http://localhost:8288
```

### Installation

**1. Clone the repository:**
 ```bash
 git clone https://github.com/Richard110206/ProductionRAGPythonApp.git
 cd ProductionRAGPythonApp
```
Create and activate a virtual environment:
```bash
python -m venv .venv

# On Windows
.venv\Scripts\activate

# On macOS/Linux
source .venv/bin/activate
```
Install dependencies using pip:
```bash
pip install .
```
Or using uv (recommended for faster installation):
```bash
uv pip install .
```
Install Inngest CLI (required for workflow management):
```bash
npm install -g inngest-cli
```
### Running the Application
The application requires three services to be running: Qdrant, Inngest server, and the application itself.

**1. Start Qdrant**
You can run Qdrant using Docker:
```bash
docker run -p 6333:6333 qdrant/qdrant
```
Or use the local storage by ensuring the qdrant_storage directory exists (already in the repository).

**2. Start Inngest server**
```bash
npx inngest-cli@latest dev -u
```
**3. Start the FastAPI backend**

Using uvicorn directly:
```bash
uvicorn main:app --reload
```
Or using uv:
```bash
uv run uvicorn main:app
```
**4. Start the Streamlit frontend**

Using streamlit directly:
```bash
streamlit run streamlit_app.py
```
Or using uv:
```bash
# On Windows
uv run streamlit run .\streamlit_app.py

# On macOS/Linux
uv run streamlit run ./streamlit_app.py
```
### Usage
1. Access the Streamlit interface at `http://localhost:8501`
2. Upload PDF files using the uploader
3. Wait for the ingestion process to complete
4. Ask questions about the content of your uploaded PDFs
5. View the AI-generated answers along with source information

### Workflow Details
**1. PDF Ingestion:**
- Uploaded PDFs are split into text chunks using SentenceSplitter (chunk size: 1000, overlap: 200)
- Text chunks are converted to embeddings using BAAI/bge-m3 model via SiliconFlow
- Embeddings are stored in Qdrant vector database with metadata
**2. Query Processing:**
- User questions are converted to embeddings
- Relevant text chunks are retrieved from Qdrant (configurable via top_k parameter)
- DeepSeek LLM generates answers based on retrieved context
### Architecture
- Frontend: Streamlit for user interaction
- Backend: FastAPI for handling API requests
- Workflow: Inngest for managing background jobs (PDF ingestion)
- Vector Database: Qdrant for storing and querying embeddings
- Embeddings: BAAI/bge-m3 model via SiliconFlow API
- LLM: DeepSeek for generating answers from retrieved contexts

## Notes
1. Do not start the server while connected to a VPN.
2. Ensure the Docker container remains running during application execution.
