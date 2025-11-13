# ProductionRAGPythonApp
![PRs 欢迎](https://img.shields.io/badge/PRs-welcome-red.svg)
![DeepSeek](https://img.shields.io/badge/LLM-DeepSeek-blueviolet.svg)
![BAAI/bge-m3](https://img.shields.io/badge/Embeddings-BAAI%2Fbge--m3-yellow.svg)
![向量维度](https://img.shields.io/badge/Vector%20Dimension-1024-blueviolet.svg)
![距离度量](https://img.shields.io/badge/Distance-Cosine-blue.svg)
![压缩方式](https://img.shields.io/badge/Compression-LZ4-green.svg)
![版本控制](https://img.shields.io/badge/VCS-Git-red.svg)

[中文版本](README_zh-CN.md) | [English Version](README.md)

一个可用于生产环境的RAG（检索增强生成）应用，支持用户上传PDF文件并通过AI查询文件中的信息。该应用专为中国大陆用户优化，解决了无法获取OpenAI API密钥的问题。如果您可以使用OpenAI API密钥，可参考此仓库：[ProductionGradeRAGPythonApp](https://github.com/techwithtim/ProductionGradeRAGPythonApp)。程序运行的详细步骤可查看YouTube频道的教学视频：[[How to Build a Production-Ready RAG AI Agent in Python (Step-by-Step)]](https://www.youtube.com/watch?v=AUQJ9eeP-Ls&t=4425s)

### 🚀 功能特性

- PDF上传与自动文本分块
- 基于Qdrant的向量存储
- 通过SiliconFlow使用BAAI/bge-m3模型生成嵌入向量
- 基于DeepSeek的AI查询回答
- 集成Inngest工作流管理的FastAPI后端
- 用于用户交互的Streamlit前端

### 📋 依赖要求

| 依赖包                   | 版本要求       | 状态徽章                                      |
|--------------------------|----------------|-----------------------------------------------|
| FastAPI                  | ≥0.116.1       | ![FastAPI](https://img.shields.io/badge/FastAPI-%3E%3D0.116.1-green.svg) |
| Streamlit                | ≥1.49.1        | ![Streamlit](https://img.shields.io/badge/Streamlit-%3E%3D1.49.1-red.svg) |
| Uvicorn                  | ≥0.35.0        | ![Uvicorn](https://img.shields.io/badge/Uvicorn-%3E%3D0.35.0-purple.svg) |
| llama-index-core         | ≥0.14.0        | ![llama-index-core](https://img.shields.io/badge/llama--index--core-%3E%3D0.14.0-yellow.svg) |
| llama-index-readers-file | ≥0.5.4         | ![llama-index-readers-file](https://img.shields.io/badge/llama--index--readers--file-%3E%3D0.5.4-blue.svg) |
| openai                   | ≥1.107.0       | ![openai](https://img.shields.io/badge/openai-%3E%3D1.107.0-blue.svg) |
| python-dotenv            | ≥1.1.1         | ![python-dotenv](https://img.shields.io/badge/python--dotenv-%3E%3D1.1.1-lightgrey.svg) |
| Python                   | ≥3.10          | ![Python](https://img.shields.io/badge/Python-%3E%3D3.10-blue.svg) |
| Qdrant Client            | ≥1.15.1        | ![Qdrant](https://img.shields.io/badge/Qdrant-Client%20%3E%3D1.15.1-indigo.svg) |
| Inngest                  | ≥0.5.6         | ![Inngest](https://img.shields.io/badge/Inngest-%3E%3D0.5.6-orange.svg) |
| Node.js                  | 必需           | ![Node.js](https://img.shields.io/badge/Node.js-Required-green.svg) |
| uv                       | 可选           | ![uv](https://img.shields.io/badge/uv-Optional-lightgrey.svg) |

### 🔧 环境配置

1. 在项目根目录创建 `.env` 文件，添加以下变量：
``` plaintext
# 文本嵌入服务的API密钥（替换为您的实际密钥）
EMBEDDING_API_KEY=your_embedding_api_key_here

# 文本嵌入服务的基础URL（根据服务提供商调整）
EMBEDDING_BASE_URL=your_embedding_service_base_url

# 要使用的嵌入模型名称（例如 BAAI/bge-m3 或其他兼容模型）
EMBED_MODEL=your_preferred_embedding_model

# 大语言模型的API密钥（替换为您的实际密钥）
LLM_API_KEY=your_llm_api_key_here

# 大语言模型服务的基础URL（根据LLM提供商调整）
LLM_BASE_URL=your_llm_service_base_url

# 要使用的LLM名称（例如 deepseek-chat 或其他兼容模型）
LLM_MODEL=your_preferred_llm_model

# Inngest服务器基础URL（本地开发通常保持为localhost）
INNGEST_API_BASE=http://localhost:8288
```
示例配置：
```
EMBEDDING_API_KEY=your_siliconflow_api_key
EMBEDDING_BASE_URL=https://api.siliconflow.cn/v1
EMBED_MODEL=BAAI/bge-m3
LLM_API_KEY=your_deepseek_api_key
LLM_BASE_URL=https://api.deepseek.com/v1
LLM_MODEL=deepseek-chat
INNGEST_API_BASE=http://localhost:8288
```
### 🛠️ 安装步骤
**1. 克隆仓库：**
```bash
git clone https://github.com/Richard110206/ProductionRAGPythonApp.git
cd ProductionRAGPythonApp
```
**2. 创建并激活虚拟环境：**
```bash
python -m venv .venv

# Windows系统
.venv\Scripts\activate

# macOS/Linux系统
source .venv/bin/activate
```
**3. 使用 pip 安装依赖：**
```bash
pip install .
```
或使用 uv 安装（推荐，安装速度更快）：
```
bash
uv pip install .
```
**4. 安装 Inngest CLI（工作流管理必需）：**
```bash
npm install -g inngest-cli
```
### ▶️ 运行应用
应用需要同时运行三个服务：Qdrant、Inngest 服务器和应用本身。

**1. 启动 Qdrant可通过 Docker 运行 Qdrant：**
```bash
docker run -p 6333:6333 qdrant/qdrant
```
或使用本地存储（确保项目中的 qdrant_storage 目录已存在）。

**2. 启动 Inngest 服务器**
```bash
npx inngest-cli@latest dev -u
```

**3. 启动 FastAPI 后端**
直接使用 uvicorn：
```bash
uvicorn main:app --reload
或使用 uv：
bash
uv run uvicorn main:app
```

**4. 启动 Streamlit 前端**
直接使用 streamlit：
```bash
streamlit run streamlit_app.py
```

或使用 uv：

```bash
# Windows系统
uv run streamlit run .\streamlit_app.py

# macOS/Linux系统
uv run streamlit run ./streamlit_app.py
```

### 📖 使用方法
1. 访问 Streamlit 界面：http://localhost:8501
2. 通过上传器上传 PDF 文件
3. 等待数据摄入过程完成
4. 询问与已上传 PDF 内容相关的问题
5. 查看 AI 生成的答案及来源信息

### 🔍 工作流详情
**1. PDF 数据摄入：**
上传的 PDF 通过 SentenceSplitter 分割为文本块（块大小：1000，重叠度：200）
文本块通过 SiliconFlow 的 BAAI/bge-m3 模型转换为嵌入向量
嵌入向量与元数据一起存储在 Qdrant 向量数据库中
**2. 查询处理：**
用户问题转换为嵌入向量
从 Qdrant 检索相关文本块（可通过 top_k 参数配置）
DeepSeek LLM 基于检索到的上下文生成答案
### 🏗️ 架构设计
- 前端：Streamlit（用户交互界面）
- 后端：FastAPI（API 请求处理）
- 工作流：Inngest（后台任务管理，如 PDF 摄入）
- 向量数据库：Qdrant（嵌入向量的存储与查询）
- 嵌入向量：通过 SiliconFlow API 调用 BAAI/bge-m3 模型生成
- 大语言模型：DeepSeek（基于检索上下文生成答案）
### ⚠️ 注意事项
1. 连接 VPN 时不要启动服务器
2. 应用运行期间确保 Docker 容器保持运行状态
### 🤝 贡献指南
欢迎贡献代码！请随时提交 Pull Request。
