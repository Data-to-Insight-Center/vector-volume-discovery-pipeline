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
