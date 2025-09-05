---
layout: default
title: "Chapter 4: Querying & Retrieval"
parent: "Chroma Tutorial"
nav_order: 4
---

# Chapter 4: Querying & Retrieval

Master the art of querying in Chroma! This chapter covers advanced querying techniques, metadata filtering, and retrieval strategies for building powerful search applications.

## Advanced Query Patterns

### Metadata Filtering

```python
# Query with metadata filters
results = collection.query(
    query_texts=["machine learning"],
    n_results=5,
    where={"category": "technology"}  # Simple equality filter
)

# Complex metadata filters
results = collection.query(
    query_texts=["python programming"],
    n_results=10,
    where={
        "$and": [
            {"difficulty": {"$in": ["beginner", "intermediate"]}},
            {"category": "programming"},
            {"rating": {"$gte": 4.0}}
        ]
    }
)

# Nested metadata queries
results = collection.query(
    query_texts=["web development"],
    where={
        "tags": {"$in": ["react", "javascript"]},
        "metadata.published": {"$eq": True}
    }
)
```

### Multi-Modal Queries

```python
# Combine text and metadata filters
hybrid_results = collection.query(
    query_texts=["artificial intelligence"],
    n_results=5,
    where={
        "category": "AI",
        "year": {"$gte": 2020}
    },
    include=["documents", "metadatas", "distances"]
)

# Temporal queries
recent_results = collection.query(
    query_texts=["latest developments"],
    where={
        "created_at": {"$gte": "2024-01-01"}
    }
)
```

## Retrieval Strategies

### Re-Ranking

```python
def rerank_results(query, initial_results, rerank_model):
    """Re-rank results using a more sophisticated model"""

    # Extract documents and scores
    documents = initial_results['documents'][0]
    scores = initial_results['distances'][0]

    # Create reranking input
    rerank_input = []
    for doc in documents:
        rerank_input.append(f"Query: {query} Document: {doc}")

    # Get reranking scores
    rerank_scores = rerank_model.predict(rerank_input)

    # Combine with original scores
    combined_scores = []
    for i, (orig_score, rerank_score) in enumerate(zip(scores, rerank_scores)):
        combined_score = 0.7 * orig_score + 0.3 * rerank_score
        combined_scores.append((i, combined_score))

    # Sort by combined score
    combined_scores.sort(key=lambda x: x[1])

    # Reorder results
    reranked_documents = [documents[i] for i, _ in combined_scores]

    return reranked_documents

# Usage
results = collection.query(query_texts=["complex query"], n_results=20)
reranked = rerank_results("complex query", results, rerank_model)
```

### Query Expansion

```python
def expand_query(original_query, expansion_model):
    """Expand query with synonyms and related terms"""

    # Generate expanded terms
    expanded_terms = expansion_model.generate_synonyms(original_query)

    # Create expanded queries
    expanded_queries = [original_query] + expanded_terms[:3]  # Limit expansion

    return expanded_queries

# Multi-query approach
expanded_queries = expand_query("machine learning", expansion_model)
results = collection.query(
    query_texts=expanded_queries,
    n_results=5
)

# Combine results from multiple queries
all_docs = []
for docs in results['documents']:
    all_docs.extend(docs)

# Remove duplicates and rerank
unique_docs = list(set(all_docs))
```

## Performance Optimization

### Query Optimization

```python
# Optimize query performance
optimized_results = collection.query(
    query_texts=["optimization techniques"],
    n_results=10,
    # Use efficient search parameters
    search_params={
        "ef": 64,  # Search quality parameter
        "k": 10    # Number of results
    }
)

# Batch queries for better performance
batch_queries = [
    "machine learning",
    "artificial intelligence",
    "data science",
    "neural networks"
]

batch_results = collection.query(
    query_texts=batch_queries,
    n_results=5
)
```

## What We've Accomplished

This chapter covered advanced querying and retrieval techniques in Chroma, including metadata filtering, re-ranking, query expansion, and performance optimization.

## Next Steps

Ready for metadata mastery? In [Chapter 5: Metadata & Filtering](05-metadata-filtering.md), we'll dive deep into advanced metadata strategies and complex filtering patterns.

---

**Practice what you've learned:**
1. Implement complex metadata filters
2. Build a re-ranking system
3. Create query expansion functionality
4. Optimize query performance for your use case

*How will you enhance your search capabilities?* 🔍
