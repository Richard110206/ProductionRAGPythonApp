## ProductionRAGPythonApp
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-red.svg)
![DeepSeek](https://img.shields.io/badge/LLM-DeepSeek-blueviolet.svg)
![BAAI/bge-m3](https://img.shields.io/badge/Embeddings-BAAI%2Fbge--m3-yellow.svg)
![Vector Dimension](https://img.shields.io/badge/Vector%20Dimension-1024-blueviolet.svg)
![Distance Metric](https://img.shields.io/badge/Distance-Cosine-blue.svg)
![Compression](https://img.shields.io/badge/Compression-LZ4-green.svg)
![VCS](https://img.shields.io/badge/VCS-Git-red.svg)

[中文版本](README_zh-CN.md) | [English Version](README.md)

A production-ready RAG (Retrieval-Augmented Generation) application that allows users to upload PDF files and query information from them using AI. It is specifically optimized for users in mainland China who may face difficulties accessing OpenAI API keys.For those who can use OpenAI API keys, you may refer to this repository:[ProductionGradeRAGPythonApp](https://github.com/techwithtim/ProductionGradeRAGPythonApp).And specific details on how to run the program are available on the YouTube channel in this video:[How to Build a Production-Ready RAG AI Agent in Python (Step-by-Step)](https://www.youtube.com/watch?v=AUQJ9eeP-Ls&t=4425s)

### 🚀 Features

- PDF upload and automatic text chunking
- Vector storage using Qdrant
- Embedding generation using BAAI/bge-m3 model via SiliconFlow
- AI-powered query answering using DeepSeek
- FastAPI backend with Inngest workflow management
- Streamlit frontend for user interaction

### 📋 Requirements


| Dependency               | Version Requirement | Status Badge                                  |
|--------------------------|---------------------|-----------------------------------------------|
| FastAPI                  | ≥0.116.1            | ![FastAPI](https://img.shields.io/badge/FastAPI-%3E%3D0.116.1-green.svg) |
| Streamlit                | ≥1.49.1             | ![Streamlit](https://img.shields.io/badge/Streamlit-%3E%3D1.49.1-red.svg) |
| Uvicorn                  | ≥0.35.0             | ![Uvicorn](https://img.shields.io/badge/Uvicorn-%3E%3D0.35.0-purple.svg) |
| llama-index-core         | ≥0.14.0             | ![llama-index-core](https://img.shields.io/badge/llama--index--core-%3E%3D0.14.0-yellow.svg) |
| llama-index-readers-file | ≥0.5.4              | ![llama-index-readers-file](https://img.shields.io/badge/llama--index--readers--file-%3E%3D0.5.4-blue.svg) |
| openai                   | ≥1.107.0            | ![openai](https://img.shields.io/badge/openai-%3E%3D1.107.0-blue.svg) |
| python-dotenv            | ≥1.1.1              | ![python-dotenv](https://img.shields.io/badge/python--dotenv-%3E%3D1.1.1-lightgrey.svg) |
| Python                   | ≥3.10               | ![Python](https://img.shields.io/badge/Python-%3E%3D3.10-blue.svg) |
| Qdrant Client            | ≥1.15.1             | ![Qdrant](https://img.shields.io/badge/Qdrant-Client%20%3E%3D1.15.1-indigo.svg) |
| Inngest                  | ≥0.5.6              | ![Inngest](https://img.shields.io/badge/Inngest-%3E%3D0.5.6-orange.svg) |
| Node.js                  | Required            | ![Node.js](https://img.shields.io/badge/Node.js-Required-green.svg) |
| uv                       | Optional            | ![uv](https://img.shields.io/badge/uv-Optional-lightgrey.svg) |

### 🔧 Environment Setup

1. Create a `.env` file in the root directory with the following variables:
``` plaintext
# API key for your text embedding service (replace with your actual key)
EMBEDDING_API_KEY=your_embedding_api_key_here

# Base URL of your text embedding service (adjust based on your provider)
EMBEDDING_BASE_URL=your_embedding_service_base_url

# Name of the embedding model to use (e.g., BAAI/bge-m3 or other compatible models)
EMBED_MODEL=your_preferred_embedding_model

# API key for your Large Language Model (replace with your actual key)
LLM_API_KEY=your_llm_api_key_here

# Base URL of your LLM service (adjust based on your LLM provider)
LLM_BASE_URL=your_llm_service_base_url

# Name of the LLM to use (e.g., deepseek-chat or other compatible models)
LLM_MODEL=your_preferred_llm_model

# Inngest server base URL (typically remains as localhost for local development)
INNGEST_API_BASE=http://localhost:8288
```
for example：
```bash
EMBEDDING_API_KEY=your_siliconflow_api_key
EMBEDDING_BASE_URL=https://api.siliconflow.cn/v1
EMBED_MODEL=BAAI/bge-m3
LLM_API_KEY=your_deepseek_api_key
LLM_BASE_URL=https://api.deepseek.com/v1
LLM_MODEL=deepseek-chat
INNGEST_API_BASE=http://localhost:8288
```

Relevant API keys are available on the following websites:

- [siliconflow official website](https://cloud.siliconflow.cn/me/models)

- [deepseek official website](https://www.deepseek.com/)

### 🛠️ Installation

**1. Clone the repository:**
 ```bash
 git clone https://github.com/Richard110206/ProductionRAGPythonApp.git
 cd ProductionRAGPythonApp
```
**2. Create and activate a virtual environment:**
```bash
python -m venv .venv

# On Windows
.venv\Scripts\activate

# On macOS/Linux
source .venv/bin/activate
```
**3. Install dependencies using pip:**
```bash
pip install .
```
Or using uv (recommended for faster installation):
```bash
uv pip install .
```
**4. Install Inngest CLI (required for workflow management):**
```bash
npm install -g inngest-cli
```
### ▶️ Running the Application
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
### 📖 Usage
1. Access the Streamlit interface at `http://localhost:8501`
2. Upload PDF files using the uploader
3. Wait for the ingestion process to complete
4. Ask questions about the content of your uploaded PDFs
5. View the AI-generated answers along with source information

### 🔍 Workflow Details
**1. PDF Ingestion:**
- Uploaded PDFs are split into text chunks using SentenceSplitter (chunk size: 1000, overlap: 200)
- Text chunks are converted to embeddings using BAAI/bge-m3 model via SiliconFlow
- Embeddings are stored in Qdrant vector database with metadata
**2. Query Processing:**
- User questions are converted to embeddings
- Relevant text chunks are retrieved from Qdrant (configurable via top_k parameter)
- DeepSeek LLM generates answers based on retrieved context
### 🏗️ Architecture
- Frontend: Streamlit for user interaction
- Backend: FastAPI for handling API requests
- Workflow: Inngest for managing background jobs (PDF ingestion)
- Vector Database: Qdrant for storing and querying embeddings
- Embeddings: BAAI/bge-m3 model via SiliconFlow API
- LLM: DeepSeek for generating answers from retrieved contexts

### ⚠️ Notes
1. Do not start the server while connected to a VPN.
2. Ensure the Docker container remains running during application execution.

### 🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

