---
layout: default
title: "Chapter 5: Metadata & Filtering"
parent: "Chroma Tutorial"
nav_order: 5
---

# Chapter 5: Metadata & Filtering

Master metadata management and advanced filtering in Chroma! This chapter covers sophisticated metadata strategies and complex filtering patterns for building powerful, precise search applications.

## Advanced Metadata Strategies

### Hierarchical Metadata Design

```python
# Design hierarchical metadata structure
hierarchical_schema = {
    "domain": "technology",
    "category": "programming",
    "subcategory": "web-development",
    "framework": "react",
    "difficulty": "intermediate",
    "tags": ["javascript", "frontend", "ui"],
    "metadata": {
        "author": "tutorial_author",
        "created": "2024-01-01",
        "version": "2.1",
        "license": "MIT",
        "rating": 4.8,
        "downloads": 1250
    }
}

# Implement hierarchical queries
def query_hierarchical(collection, domain, category=None, tags=None):
    query_conditions = {"domain": domain}

    if category:
        query_conditions["category"] = category

    if tags:
        query_conditions["$or"] = [
            {"tags": {"$in": tags}},
            {"tags": {"$contains": tags[0]}}
        ]

    return collection.query(
        query_texts=[""],
        where=query_conditions,
        n_results=20
    )
```

### Dynamic Metadata

```python
# Add computed metadata dynamically
def add_computed_metadata(documents, metadatas):
    enhanced_metadatas = []

    for doc, metadata in zip(documents, metadatas):
        enhanced = {**metadata}

        # Add computed fields
        enhanced["word_count"] = len(doc.split())
        enhanced["has_code"] = "```" in doc
        enhanced["reading_time"] = max(1, enhanced["word_count"] // 200)  # ~200 words per minute
        enhanced["complexity_score"] = calculate_complexity(doc)

        enhanced_metadatas.append(enhanced)

    return enhanced_metadatas

def calculate_complexity(text):
    # Simple complexity heuristic
    technical_terms = ["algorithm", "function", "class", "async", "api"]
    return sum(1 for term in technical_terms if term.lower() in text.lower())
```

## Complex Filtering Patterns

### Multi-Condition Filters

```python
# Advanced filtering with multiple conditions
complex_filters = {
    # Technology stack filter
    "tech_stack": {
        "$and": [
            {"tags": {"$in": ["javascript", "typescript"]}},
            {"framework": {"$in": ["react", "vue", "angular"]}},
            {"difficulty": {"$ne": "expert"}}
        ]
    },

    # Quality and popularity filter
    "high_quality": {
        "$and": [
            {"rating": {"$gte": 4.5}},
            {"downloads": {"$gte": 1000}},
            {"metadata.version": {"$regex": r"^[^0]"}},  # Not starting with 0
            {"created": {"$gte": "2023-01-01"}}
        ]
    },

    # Content type filter
    "tutorials_only": {
        "$and": [
            {"category": "tutorial"},
            {"word_count": {"$gte": 500}},
            {"has_code": True},
            {"reading_time": {"$lte": 30}}  # Under 30 minutes
        ]
    }
}

# Apply complex filters
results = collection.query(
    query_texts=["web development"],
    where=complex_filters["tech_stack"],
    n_results=10
)
```

### Temporal and Range Filters

```python
# Time-based filtering
temporal_filters = {
    "recent": {
        "created": {"$gte": "2024-01-01"}
    },
    "this_month": {
        "created": {"$gte": "2024-01-01", "$lt": "2024-02-01"}
    },
    "last_week": {
        "created": {"$gte": "2024-01-24"}
    }
}

# Range-based filtering
range_filters = {
    "popular": {
        "downloads": {"$gte": 1000, "$lte": 10000}
    },
    "highly_rated": {
        "rating": {"$gte": 4.5}
    },
    "medium_length": {
        "word_count": {"$gte": 500, "$lte": 2000}
    }
}
```

## Metadata Indexing and Performance

### Index Optimization

```python
# Optimize metadata for faster filtering
def optimize_metadata_index(collection, frequently_queried_fields):
    # Create composite indexes for common query patterns
    index_configs = {
        "tech_filter": ["tags", "framework", "difficulty"],
        "quality_filter": ["rating", "downloads", "created"],
        "content_filter": ["category", "word_count", "has_code"]
    }

    for index_name, fields in index_configs.items():
        # In a real implementation, this would create database indexes
        print(f"Optimizing index for: {', '.join(fields)}")

# Usage
frequent_fields = ["tags", "category", "rating", "created"]
optimize_metadata_index(collection, frequent_fields)
```

## What We've Accomplished

This chapter covered advanced metadata management and complex filtering patterns in Chroma, including hierarchical metadata, dynamic computed fields, complex multi-condition filters, and performance optimization.

## Next Steps

Ready for integration? In [Chapter 6: Integration Patterns](06-integration-patterns.md), we'll explore how to integrate Chroma with popular AI frameworks and applications.

---

**Practice what you've learned:**
1. Design hierarchical metadata for your domain
2. Implement complex multi-condition filters
3. Add computed metadata fields
4. Optimize metadata indexing for performance

*How will you structure your metadata?* 📊
