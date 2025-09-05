---
layout: default
title: "Chapter 6: Integration Patterns"
parent: "Chroma Tutorial"
nav_order: 6
---

# Chapter 6: Integration Patterns

Learn how to integrate Chroma with popular AI frameworks and applications! This chapter covers integration patterns with LangChain, LlamaIndex, and other AI tools.

## LangChain Integration

### VectorStore Integration

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings
from langchain.text_splitter import RecursiveCharacterTextSplitter

# Create Chroma vectorstore
embeddings = OpenAIEmbeddings()
vectorstore = Chroma(
    collection_name="langchain_docs",
    embedding_function=embeddings
)

# Add documents
texts = ["LangChain documentation", "Chroma integration guide"]
vectorstore.add_texts(texts)

# Query
results = vectorstore.similarity_search("integration patterns", k=3)
```

### RetrievalQA Chain

```python
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI

# Create QA chain with Chroma
qa_chain = RetrievalQA.from_chain_type(
    llm=OpenAI(),
    chain_type="stuff",
    retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
    return_source_documents=True
)

# Ask questions
result = qa_chain({"query": "How does Chroma integrate with LangChain?"})
print(result["result"])
```

## LlamaIndex Integration

### Vector Store Integration

```python
from llama_index import VectorStoreIndex, SimpleDirectoryReader
from llama_index.vector_stores import ChromaVectorStore
from llama_index.storage import StorageContext

# Create Chroma vector store
chroma_store = ChromaVectorStore(chroma_collection=collection)

# Create storage context
storage_context = StorageContext.from_defaults(vector_store=chroma_store)

# Create index
index = VectorStoreIndex.from_documents(
    documents=documents,
    storage_context=storage_context
)

# Query
query_engine = index.as_query_engine()
response = query_engine.query("What is Chroma?")
```

## What We've Accomplished

This chapter covered integration patterns with popular AI frameworks, focusing on LangChain and LlamaIndex integration with Chroma.

## Next Steps

Ready for production? In [Chapter 7: Production Deployment](07-production-deployment.md), we'll cover scaling Chroma for production workloads.

---

**Practice what you've learned:**
1. Integrate Chroma with LangChain
2. Build a LlamaIndex application with Chroma
3. Create retrieval-augmented generation pipelines
4. Implement multi-framework integrations

*How will you integrate Chroma into your AI stack?* 🔗
