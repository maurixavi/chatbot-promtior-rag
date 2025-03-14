# RAG-Based Chatbot

## Overview
This project implements a **Retrieval-Augmented Generation (RAG) chatbot** using **LangChain**, **ChatGroq** with the **Llama3-8b-8192 model**, and **Streamlit** for the user interface. The chatbot leverages **Hugging Face embeddings** to retrieve relevant context from structured data sources, which is then used by ChatGroq inference capabilities to generate accurate and context-aware responses to user queries. The backend is exposed via **LangServe** through FastAPI endpoints, enabling seamless communication between the frontend and backend. The entire application is deployed on **AWS EC2**, where both the server and client are hosted on a single instance.

## Features
- **Retrieval-Augmented Generation (RAG) model**
- **Session-based conversation history**
- **Data ingestion from the Promtior website and PDF files**
- **Fine-tuned text chunking and embedding generation**
- **REST API with LangServe**
- **Interactive frontend with Streamlit**
- **Deployed on AWS EC2**

## Installation (Local Setup)
### Prerequisites
Ensure you have the following installed:
- Python 3.9+
- Pip
- Virtual environment
- Git

### 1. Clone the Repository
```sh
git clone https://github.com/maurixavi/chatbot-promtior-rag
cd rag-chatbot
```

### 2. Create a Virtual Environment (Optional)
```sh
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```

### 3. Install Dependencies
```sh
pip install -r requirements.txt
```

### 4. Start the Server
Run the LangServe-based API server:
```sh
python server.py
```

### 5. Start the Frontend
Run the Streamlit frontend:
```sh
streamlit run chatbot.py
```

The chatbot should now be accessible at `http://localhost:8501/`.

---

## Deployment on AWS EC2
### 1. Set Up an EC2 Instance
- Choose an **Ubuntu 22.04 LTS** instance
- Configure security groups to allow:
  - **Port 8501** (Streamlit UI)
  - **Port 8000** (API Server)

### 2. Connect to Your Instance
```sh
ssh -i your-key.pem ubuntu@your-ec2-instance-ip
```

### 3. Install Dependencies on EC2
```sh
sudo apt update && sudo apt install -y python3-pip python3-venv git
```

### 4. Clone the Repository on EC2
```sh
git clone https://github.com/maurixavi/chatbot-promtior-rag
cd rag-chatbot
```

### 5. Set Up the Virtual Environment and Install Dependencies
```sh
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 6. Run the Server
```sh
nohup uvicorn server:app --host 0.0.0.0 --port 8000 --reload &
```

### 7. Run the Frontend
```sh
nohup streamlit run chatbot.py --server.port 8001 &
```

### 8. Access the Chatbot
Open your browser and go to:
```
http://your-ec2-instance-ip:8501/
```
