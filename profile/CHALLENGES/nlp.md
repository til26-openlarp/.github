# NLP Challenge — Natural Language Processing (RAG QA)

Build a Retrieval-Augmented Generation (RAG) question-answering system.

## Overview

- **Type:** Question Answering (RAG)
- **Task:** Answer questions using provided documents
- **Port:** 5004
- **Difficulty:** Advanced (requires LLM knowledge)
- **Training Time:** Minimal (mostly implementation)

## Challenge Interface

### Step 1: Load Documents
```json
{
  "instances": [
    {
      "documents": [
        "Document 1 text...",
        "Document 2 text...",
        ...
      ]
    }
  ]
}
```

**Response:** Must return `{"predictions": ["loaded"]}`

### Step 2: Answer Questions
```json
{
  "instances": [
    {"question": "What is X?"},
    {"question": "How does Y work?"}
  ]
}
```

**Response:**
```json
{
  "predictions": [
    "Answer to question 1",
    "Answer to question 2"
  ]
}
```

## What is RAG?

RAG = Retrieval + Generation

1. **Retrieval:** Search documents for relevant passages
2. **Generation:** Use LLM to generate answer based on retrieved passages

**vs. Fine-tuning:** RAG is faster, more flexible, better for custom knowledge

## Implementation Strategy

### Option 1: LangChain (Recommended)
```python
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import FAISS
from langchain.llms import HuggingFaceLLM
from langchain.chains import RetrievalQA

# Load documents
embeddings = HuggingFaceEmbeddings()
vectorstore = FAISS.from_documents(docs, embeddings)

# Create QA chain
qa = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=vectorstore.as_retriever()
)

# Answer
answer = qa.run(question)
```

### Option 2: Custom Implementation
```python
class RAGManager:
    def load_documents(self, documents):
        # Chunk documents
        # Embed chunks
        # Store in vector DB (FAISS, Milvus, etc.)
        pass
    
    def answer_question(self, question):
        # Embed question
        # Retrieve relevant chunks
        # Call LLM with context
        # Return answer
        pass
```

## Key Components

### 1. Embedding Model
- **Popular:** sentence-transformers (fast, good quality)
- **Size:** 384-1024 dimensions
- **Examples:**
  - `sentence-transformers/all-MiniLM-L6-v2` (fast, 384-dim)
  - `sentence-transformers/all-mpnet-base-v2` (better, 768-dim)

### 2. Vector Store
- **FAISS:** In-memory, fast (local)
- **Milvus:** Distributed, scalable
- **Pinecone:** Cloud-based
- **Chroma:** Simple, local

### 3. Language Model
- **Open:** Llama, Mistral, Zephyr
- **API:** GPT-4, Claude, Cohere

## Quick Start

```bash
cd nlp
pip install -r train/requirements.txt

# Create RAG system
python -c "
import langchain
from langchain.embeddings import HuggingFaceEmbeddings
embeddings = HuggingFaceEmbeddings()
print('✓ Ready to build RAG')
"
```

## Implementation Checklist

- [ ] Load documents (chunking if needed)
- [ ] Create embeddings
- [ ] Build vector store
- [ ] Implement retrieval (top-k similar chunks)
- [ ] Integrate LLM (OpenAI, HuggingFace, etc.)
- [ ] Generate answers with context
- [ ] Handle edge cases (no relevant chunks, etc.)
- [ ] Test with sample questions
- [ ] Deploy as FastAPI service
- [ ] Test on port 5004

## Performance Considerations

### Speed
- Use smaller embedding model (all-MiniLM-L6-v2)
- Limit retrieval to top-3 chunks
- Cache embeddings
- Use quantized LLM

### Quality
- Use larger embedding model (all-mpnet-base-v2)
- Retrieve top-10 chunks
- Use better LLM (GPT-4 > Mistral > Llama)
- Add prompt engineering

## Advanced Techniques

### Retrieval Enhancement
- **Dense + Sparse:** BM25 + semantic search
- **Reranking:** Use cross-encoder to rerank results
- **Hybrid Search:** Combine multiple methods

### Generation Enhancement
- **Chain-of-Thought:** Ask model to reason step-by-step
- **Multi-turn:** Keep conversation history
- **Fact-checking:** Verify against source documents

## Key Files

- `nlp/README.md` — Challenge overview
- `nlp/AGENTS.md` — Implementation guide
- `nlp/submit/README.md` — Submission setup
- `nlp/submit/src/nlp_server.py` — FastAPI server
- `nlp/submit/src/nlp_manager.py` — RAG implementation
- `nlp/train/requirements.txt` — Dependencies

## Dependencies

Essential:
- `langchain` — RAG framework
- `sentence-transformers` — Embeddings
- `faiss-cpu` — Vector store
- `transformers` — Model hub

Optional:
- `openai` — For GPT models
- `torch` — For local LLMs
- `chromadb` — Alternative vector store

## Testing

### Local Testing
```bash
cd nlp/submit/src
uvicorn nlp_server:app --port 5004

# In another terminal:
curl -X POST http://localhost:5004/nlp \
  -H "Content-Type: application/json" \
  -d '{
    "instances": [{
      "documents": ["Sample doc"],
      "question": "What is in the document?"
    }]
  }'
```

## Submission Checklist

- [ ] RAG system loads documents correctly
- [ ] Returns "loaded" after document load
- [ ] Answers questions on port 5004
- [ ] Handles multiple questions in one request
- [ ] Returns answers in correct JSON format
- [ ] Handles edge cases (empty docs, no results, etc.)
- [ ] Tested locally
- [ ] Docker builds without errors
- [ ] Container starts and responds

## Common Issues

| Issue | Solution |
|-------|----------|
| Out of memory | Use smaller embedding model |
| Slow retrieval | Reduce chunk count, use smaller LLM |
| Bad answers | Better chunks, improve prompt, use better LLM |
| "loaded" response fails | Check document processing |
| Server won't start | Check import errors, missing deps |

## Resources

- **LangChain docs:** https://python.langchain.com/
- **Embeddings:** https://huggingface.co/sentence-transformers
- **Open LLMs:** https://huggingface.co/models?task=text-generation
- **FAISS guide:** https://github.com/facebookresearch/faiss

## Next Steps

1. Read `../nlp/README.md`
2. Read `../nlp/AGENTS.md` (detailed guide)
3. Choose embedding + LLM combination
4. Implement RAG manager
5. Test locally
6. Deploy to submission server
7. Iterate!

---

**Estimated time to first submission:** 6-8 hours
