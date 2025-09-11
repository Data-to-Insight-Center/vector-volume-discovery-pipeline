# vector-volume-discovery-pipeline

The **Vector Volume Discovery Pipeline** is a scalable, multimodal retrieval system for discovering content in document images using vector embeddings. This system uses the **ColPali model**, which combines SigLIP image patch embeddings with a Gemma-2B language model, to generate **ColBERT-style multi-vector representations** of document pages. These vectors are stored and queried via a **Qdrant** vector database.

This repository includes code to host the models, generate embeddings, and run a backend FastAPI server for retrieval and generation tasks.

---

##  Components

- **colpali_standalone/** – Scripts to host ColPali model and embedding functions
- **llm_vision_models/** – Hosting scripts for LLaMA 3.2 Vision and Paligemma for response generation
- **backend/** – FastAPI server that connects to all models and Qdrant for inference, search, and generation
- **Colpali_Image_Embeddings_v1.ipynb** – Colab/Notebook version for testing embedding workflows

---

## Hosting the Models

### Prerequisites

- Python 3.8+
- Pip
- Qdrant running locally or remotely
- Access to a GPU (recommended) for model inference

### 1. Clone the Repository


```bash
git clone <repository-url>
cd vector-volume-discovery-pipeline
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```


### 3. Host Each Model

Navigate to the respective directories and run the hosting scripts.

#### i. ColPali Model (Image Embeddings)

```bash
cd colpali_standalone
python -m uvicorn colpali_host_script:app --host 0.0.0.0 --port 8000 --reload
```

#### ii. LLaMA 3.2 Vision

```bash
cd llm_vision_models
python -m uvicorn llama_3_2_vision_host_script:app --host 0.0.0.0 --port 8000 --reload
```

#### iii. Paligemma

```bash
cd llm_vision_models
python -m uvicorn paligemma_host_script:app --host 0.0.0.0 --port 8000 --reload
```

---

## Backend – FastAPI Server

The backend module powers inference workflows by connecting to the hosted models and Qdrant.

---

###  Structure

```
backend/
├── api_router.py                # Central FastAPI router for model & Qdrant interaction
├── colpali_client.py           # API client to interact with hosted ColPali model
├── llama_client.py             # API client for LLaMA 3.2 Vision
├── paligemma_client.py         # API client for Paligemma model
├── qdrant_client.py            # Functions to upsert, search, and manage vectors in Qdrant
├── utils.py                    # Image preprocessing, base64 conversion, and helper functions
└── main.py                     # FastAPI entrypoint for backend server
```

---

### Run the Backend Server

```bash
cd backend
uvicorn main:app --host 0.0.0.0 --port 8080 --reload
```

---

##  API Endpoints

### `POST /embed`
- Sends a document image to ColPali  
- Returns multi-vector embedding and optionally inserts into Qdrant

### `POST /search`
- Accepts a query vector or image  
- Returns top-K most relevant document pages from Qdrant

### `POST /generate`
- Sends retrieved pages to LLaMA 3.2 Vision or Paligemma  
- Returns generated answer based on retrieved context

---

##  Environment Variables

Optional `.env` file configuration:

```
COLPALI_API_URL=http://localhost:8000
LLAMA_API_URL=http://localhost:8000
PALIGEMMA_API_URL=http://localhost:8000
QDRANT_URL=http://localhost:6333
QDRANT_COLLECTION=vector_volume_pages
```
