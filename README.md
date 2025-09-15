# vector-volume-discovery-pipeline

The Vector Volume Discovery Pipeline is a multimodal retrieval system for searching document images using dense vector similarity. It encodes each page into ColBERT-style multi-vector embeddings and stores them in Qdrant for fast, semantic search. The system supports both image and text queries and routes top-K results through vision-language models for context-aware generation. This project also benchmarks four similarity functions - cosine, dot product, euclidean, and manhattan - to evaluate retrieval quality across real-world textbook queries.
This repository includes code to host the models, generate embeddings, and run a backend FastAPI server for retrieval and generation tasks.

---

##  Components

- **colpali_standalone/** – Scripts to host ColPali model and embedding functions
- **llm_vision_models/** – Hosting scripts for LLaMA 3.2 Vision and Paligemma for response generation
- **backend/** – FastAPI server that connects to all models and Qdrant for inference, search, and generation
- **Colpali_Image_Embeddings_v1.ipynb** – Colab/Notebook version for testing embedding workflows
- 

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
