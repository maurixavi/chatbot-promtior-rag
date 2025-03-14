# **Project Overview**

To tackle the challenge of implementing a RAG-based chatbot, I followed a practical, step-by-step approach. I began by researching the key concepts of RAG through the official LangChain documentation, which helped me break the problem into two main parts: data ingestion and indexing, and retrieval and response generation. I implemented an initial version in a Jupyter Notebook to familiarize myself with the workflow, allowing me to identify key steps and potential technical challenges.

Once I had a solid understanding of the concepts, I developed a basic chatbot using Streamlit and Groq’s chat model, focusing on conversation management and memory handling. This gave me the confidence to move toward a more complex solution incorporating RAG. I integrated data ingestion from the Promptior website and PDF files, generated embeddings using a Hugging Face model, and configured a RAG chain to retrieve relevant documents and generate responses.

When it came to integrating LangServe, my first step was to explore the official documentation and experiment with the provided example. This helped me understand how LangServe handles requests and responses and how to expose the RAG chain as a REST API. The main challenge was configuring the RAG chain to work with the chat endpoint while maintaining conversation memory. The tutorial didn’t cover this, so I researched and discovered **RunnableWithMessageHistory**, which turned out to be the perfect solution. This allowed the chatbot to remember previous interactions and generate more coherent responses.

Finally, I deployed the complete application on AWS by cloning a single repository and exposing both the server and the client on a single AWS EC2 instance.

---

## **Implementation Logic**

The chatbot implementation was divided into several key stages:

- **Data Ingestion:** Used **WebBaseLoader** to load documents from the Promptior website and **PyPDFLoader** to handle PDF files. These documents were combined and split into smaller chunks using **RecursiveCharacterTextSplitter**.
- **Embedding Creation:** Generated embeddings using **sentence-transformers/all-MiniLM-L6-v2**, a Hugging Face model based on **MiniLM**, offering a good balance between performance and speed.
- **RAG Chain Configuration:** Implemented a **RAG chain** combining a **retriever** (to fetch relevant documents) and a **generator** (to produce responses based on the retrieved documents) using **Groq’s chat model**.
- **Integration with LangServe:** Integrated the conversational **RAG chain** into **FastAPI**, exposing the chatbot as a REST endpoint.

---

## **Main Challenges and Solutions**

- **Context and Memory Management:**  
  - The main challenge was configuring the **rag_chain** to work with **add_routes** while maintaining conversation memory.
  - The tutorial didn’t cover this, so I researched and discovered **RunnableWithMessageHistory**, which turned out to be the perfect solution.
  - With **RunnableWithMessageHistory**, I wrapped the **rag_chain** and managed session-based conversation history.
  - This allowed the chatbot to remember previous interactions and generate coherent responses.
  - Finally, I integrated the **rag_chain** into **FastAPI** using **add_routes**, exposing the chatbot as a REST endpoint (`/chat`).

- **Retrieving Relevant Context:**  
  - Initially, the chatbot struggled to retrieve the correct context for answering specific questions like _"What services does Promptior offer?"_ or _"When was the company founded?"_.
  - To solve this, I fine-tuned the parameters of **RecursiveCharacterTextSplitter** through trial and error.
  - This improved text chunk quality, ensuring the retriever could find relevant information.
  - Additionally, I experimented with different prompts, searching for more specialized options rather than using generic recommendations.

- **Choosing Embeddings:**  
  - Although the documentation suggested **Ollama** and **LLaMA2**, I opted for a more lightweight and efficient **Hugging Face model**.
  - However, when deploying the application on AWS, I encountered **disk space issues** due to large dependencies from **langchain_huggingface**.
  - To resolve this, I switched to **HuggingFaceEndpointEmbeddings**, which uses an **API** instead of installing models locally.
  - This allowed me to install the required libraries on the AWS instance without disk space problems.

- **Deployment on AWS:**  
  - Another challenge was deploying the application on **AWS**, as I had no prior experience with the platform.
  - By following tutorials, I was able to find the necessary tools and services to successfully deploy the application.

---

## **Technologies Used**

- **LangChain** – For implementing the RAG chain and managing conversation history.
- **FastAPI** – For exposing the chatbot as a REST API.
- **Streamlit** – For the chatbot’s user interface.
- **Hugging Face** – For embedding generation.
- **Groq** – For response generation using its chat model.
- **AWS EC2** – For deploying the chatbot in a cloud environment.

---

## **Deployment**

For deployment, I used an **AWS EC2 instance**. Following tutorials and documentation, I:

- Set up the EC2 instance.
- Cloned the GitHub repository.
- Ran the application as background processes for both the **FastAPI server** and the **Streamlit interface**.
- Exposed the chatbot on a **free domain provided by AWS**, making the solution publicly accessible.
