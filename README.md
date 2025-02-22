# Qdrant RAG Research Agent

An intelligent retrieval and research agent that leverages the power of Qdrant for indexing and vector-based search. This project is built using LangGraph and LangChain, and it is designed to index documents and answer user queries by retrieving the most relevant information from a Qdrant-powered vector database.

> **Created by Ashutosh Upadhyay (Cognio Labs)**

---

## Overview

This agent provides a simple yet powerful system for:
- **Indexing**: Documents are parsed and stored in Qdrant as vectors.
- **Retrieval**: Given a user query, the agent retrieves relevant documents using vector similarity search.
- **Response Generation**: A language model (e.g., Anthropic's Claude or OpenAI's GPT) generates comprehensive answers based on the retrieved context.

The entire system now exclusively uses Qdrant as the vector store for both indexing and retrieval.

---

## Features

- **Qdrant-Backed Indexing:** Store and index documents in a Qdrant collection for efficient search.
- **Vector Search Retrieval:** Use cosine similarity to fetch relevant documents based on user queries.
- **Extensible Research Workflow:** Built on LangGraph, the agent supports multi-step research plans and context-aware conversation.
- **Easy Configuration:** Set up using environment variables and configuration files.
- **LLM Integration:** Generate final responses using popular language models.

---

## Prerequisites

Before getting started, ensure you have the following:

- **Python 3.9+**: Required for running the project.
- **Qdrant Instance:**  
  - **Local Setup (Docker):**  
    ```bash
    docker run -p 6333:6333 -v $(pwd)/qdrant_storage:/qdrant/storage qdrant/qdrant
    ```
  - **Cloud Setup:** Alternatively, sign up for [Qdrant Cloud](https://cloud.qdrant.io/) and obtain your API key.

- **API Keys for LLMs:** (Optional) If you wish to use a language model for generating responses, set up an account with providers such as Anthropic or OpenAI and obtain the necessary API keys.

---

## Installation

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/yourusername/rag-research-graph.git
   cd rag-research-graph
   ```

2. **Install Dependencies:**

   It is recommended to work in a virtual environment:

   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows: venv\Scripts\activate
   pip install -e .
   ```

   The Qdrant client along with other necessary packages will be installed.

---

## Configuration

Create a `.env` file in the project root with the following content (adjust values as needed):

```dotenv
# Qdrant configuration
QDRANT_URL=http://localhost:6333
# QDRANT_API_KEY is optional for local deployments; required for Qdrant Cloud
QDRANT_API_KEY=your_qdrant_api_key

# LLM API Keys (if using an LLM for response generation)
ANTHROPIC_API_KEY=your_anthropic_api_key
OPENAI_API_KEY=your_openai_api_key
```

The project's configuration is set to use Qdrant by default. In `src/shared/configuration.py`, the `retriever_provider` is now set to `"qdrant"`.

---

## Using the Agent

### Indexing Documents

The indexing graph will ingest documents and store them as vectors in Qdrant. To index documents:

1. **Prepare Your Documents:**

   Documents must follow a JSON format like:
   ```json
   [
     {
       "page_content": "LangGraph and LangChain enable stateful and multi-agent workflows for AI."
     }
   ]
   ```

   Alternatively, if you provide an empty list, the agent will load sample documents.

2. **Run the Indexing Graph:**

   Use LangGraph Studio or invoke the indexer directly:
   ```bash
   python -m langgraph.run --graph indexer
   ```

   The indexer collection (`langchain_collection`) will be created in Qdrant and documents will be inserted.

### Retrieving and Answering Queries

After the documents are indexed, use the retrieval graph to handle user queries:

1. **Switch to the Retrieval Graph in LangGraph Studio** (or run it directly):
   ```bash
   python -m langgraph.run --graph retrieval_graph
   ```

2. **Enter Your Query:**

   The agent will:
   - Use Qdrant to retrieve relevant documents.
   - Generate a research plan if needed.
   - Produce a final response based on search results and conversation history.

---

## Development and Customization

- **Graph Customization:**  
  Update the nodes in `src/index_graph` and `src/retrieval_graph` to customize how indexing, retrieval, or response generation works.

- **Prompts and LLM Settings:**  
  Modify the prompts in `src/retrieval_graph/prompts.py` to adjust the agent's behavior.  
  Update LLM model names in the configuration if you want to switch providers (refer to the LLM API documentation for instructions).

- **Testing:**  
  Run unit and integration tests using:
  ```bash
  make test
  make integration_tests
  ```

---

## Credits

This agent was created by **Ashutosh Upadhyay from Cognio Labs**. It demonstrates how to integrate a Qdrant vector database with LangGraph and LangChain for performing retrieval-augmented generation (RAG).

---

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
