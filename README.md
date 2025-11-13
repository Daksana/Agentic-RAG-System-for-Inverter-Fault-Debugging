# Agentic RAG System for Inverter Fault Debugging

AI-powered inverter troubleshooting system using intelligent routing, hybrid retrieval (semantic search with reranking), and LLM-based reasoning for fault diagnosis and specification lookup.

## Features

- **Intelligent Query Classification**: LLM automatically routes queries to the right knowledge source (error codes, datasheets, or real-time tools) based on user intent

- **Multi-Source Retrieval System**: Three-tier routing strategy for comprehensive troubleshooting:
  1. Error codes database for fault diagnostics
  2. Technical datasheet for specifications and features
  3. Real-time tool integration for live inverter metrics
  
- **Hybrid Search with Reranking**: Combines semantic search using Google text-embedding-004 with Cohere Rerank v3.5 for highly relevant context retrieval

- **Semantic Chunking**: Uses intelligent document splitting that preserves context boundaries rather than arbitrary character limits

- **Context-Aware Answers**: Generates detailed responses with inline source citations (page and document references) for easy verification

## Getting Started

### 1. Install dependencies:
```bash
pip install langchain langchain-community langchain-google-genai google-generativeai faiss-cpu pypdf cohere langchain-experimental
```

### 2. Set up API keys:
```python
GEMINI_API_KEY = "your_gemini_key_here"
COHERE = "your_cohere_key_here"
```

### 3. Prepare data files:
- `Datasheet.pdf` - Technical specifications and features of the inverter
- `EventCodes.pdf` - Complete error code definitions and troubleshooting guides

### 4. Run:
```python
# Execute notebook cells sequentially
# Pipeline: Load PDFs → Chunk documents → Build vector stores → Route queries → Retrieve and answer
```

## How It Works

1. **Load and chunk PDFs** → Semantic chunking splits documents intelligently while preserving context
2. **Build vector databases** → Create FAISS indexes with Google embeddings for fast similarity search
3. **Classify user query** → LLM router determines if question is about errors, specs, or needs real-time data
4. **Smart retrieval** → System applies the right strategy:
```
IF query about fault/error → Search error codes database
ELIF query about specs/features → Search datasheet database
ELIF query about current power → Call inverter API tool
ELSE → Ask user to clarify intent
```
5. **Rerank results** → Cohere reranker selects the most relevant chunks from initial retrieval
6. **Generate answer** → LLM synthesizes response with inline citations to source documents

## Example Input

```python
# Fault diagnosis
"What does error code 501 mean?"

# Specification lookup
"What is the AC output voltage specification?"

# Real-time data
"What is current AC power of inverter IS_1655?"
```

## Example Output

```
Route: error_codes | Inverter ID: None

=== ANSWER ===
Error code 501 indicates a Grid Overvoltage fault [EventCodes.pdf:page 3]. This occurs when the grid voltage exceeds the maximum threshold of 264V AC. The inverter automatically disconnects to protect the system. Check your local grid voltage and ensure it is within the acceptable range of 180-264V AC [Datasheet.pdf:page 5].

=== TOP CONTEXT ===
[1] source=EventCodes.pdf page=3
[2] source=EventCodes.pdf page=3
[3] source=Datasheet.pdf page=5
```

*Project developed under AI Orchestration Bootcamp by Senzmate IoT Intelligence*
