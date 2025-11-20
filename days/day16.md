# Day 16: Semantic Search Implementation

## =Ì Objective
Build a practical semantic search system that uses embeddings to find relevant documents, implementing a real-world solution for intelligent information retrieval.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- OpenAI API key
- Python 3.8+
- `pip install openai python-dotenv numpy scikit-learn`

### Part 1: Understanding Semantic Search (2 min)

**Traditional Search vs Semantic Search**:

```
Keyword Search:
User: "How to fix laptop?"
Results: Only documents containing "fix" AND "laptop"
Problem: Misses "repair computer" or "troubleshoot device"

Semantic Search:
User: "How to fix laptop?"
Results: Documents about fixing, repairing, troubleshooting
         + documents about laptops, computers, devices
         + combines meaning, not just keywords
```

**How Semantic Search Works**:
```
1. Create embeddings for all documents
2. Create embedding for search query
3. Find documents with highest similarity to query
4. Rank by similarity score
5. Return top results
```

### Part 2: Build a Semantic Search System (10 min)

**Create `semantic_search.py`**:

```python
import os
import json
from openai import OpenAI
from dotenv import load_dotenv
import numpy as np

load_dotenv()
client = OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

class SemanticSearchEngine:
    """Simple semantic search implementation"""

    def __init__(self):
        self.documents = {}
        self.embeddings = {}
        self.model = "text-embedding-3-small"

    def add_document(self, doc_id, text):
        """Add a document to the search engine"""
        self.documents[doc_id] = text
        print(f"Added document: {doc_id}")

    def build_index(self):
        """Create embeddings for all documents"""
        print("\nBuilding search index...")

        for doc_id, text in self.documents.items():
            response = client.embeddings.create(
                model=self.model,
                input=text
            )
            self.embeddings[doc_id] = response.data[0].embedding
            print(f" Indexed: {doc_id}")

    def search(self, query, top_k=3):
        """Search for documents similar to query"""

        # Get query embedding
        response = client.embeddings.create(
            model=self.model,
            input=query
        )
        query_embedding = response.data[0].embedding

        # Calculate similarities
        results = []
        for doc_id, doc_embedding in self.embeddings.items():
            # Cosine similarity
            similarity = self._cosine_similarity(
                query_embedding,
                doc_embedding
            )
            results.append((doc_id, similarity))

        # Sort by similarity (descending)
        results.sort(key=lambda x: x[1], reverse=True)

        return results[:top_k]

    @staticmethod
    def _cosine_similarity(vec1, vec2):
        """Calculate cosine similarity"""
        v1 = np.array(vec1)
        v2 = np.array(vec2)
        return np.dot(v1, v2) / (np.linalg.norm(v1) * np.linalg.norm(v2))


# Example: Create a knowledge base
print("="*60)
print("SEMANTIC SEARCH ENGINE DEMO")
print("="*60)

engine = SemanticSearchEngine()

# Add documents
documents = {
    "python_basics": """Python is a high-level programming language known for its
        simplicity and readability. It uses indentation for code blocks and supports
        multiple programming paradigms including object-oriented, functional, and
        procedural programming.""",

    "web_dev": """Web development involves building websites and web applications.
        Common tools include HTML for structure, CSS for styling, and JavaScript for
        interactivity. Popular frameworks include React, Vue, and Angular.""",

    "ml_intro": """Machine learning is a subset of artificial intelligence that focuses
        on training computers to learn from data. Common algorithms include decision
        trees, neural networks, and support vector machines.""",

    "databases": """Databases store and organize data for efficient retrieval. SQL
        databases use structured tables, while NoSQL databases like MongoDB use
        flexible document formats. Redis is used for caching.""",

    "ai_ethics": """AI ethics addresses the responsible development and deployment of
        artificial intelligence systems. Key concerns include bias, transparency,
        accountability, and fairness in algorithmic decision-making."""
}

# Add all documents
for doc_id, content in documents.items():
    engine.add_document(doc_id, content)

# Build search index
engine.build_index()

# Test queries
queries = [
    "How to learn programming?",
    "Building web applications",
    "Machine learning algorithms",
    "Data storage solutions",
    "Ethical concerns in AI"
]

print("\n" + "="*60)
print("SEARCH RESULTS")
print("="*60)

for query in queries:
    print(f"\nQuery: '{query}'")
    print("-" * 40)

    results = engine.search(query, top_k=2)

    for rank, (doc_id, similarity) in enumerate(results, 1):
        print(f"{rank}. {doc_id}")
        print(f"   Relevance: {similarity:.4f}")
        print(f"   Content: {documents[doc_id][:80]}...")
```

**Run it**:
```bash
python semantic_search.py
```

### Part 3: Advanced: Using Pinecone (Optional, 3 min)

**Pinecone Cloud Vector Database** (if you want to scale):

```bash
pip install pinecone-client
```

```python
from pinecone import Pinecone
import os

# Initialize Pinecone
pc = Pinecone(api_key=os.getenv("PINECONE_API_KEY"))
index = pc.Index("semantic-search")

# Upsert embeddings
index.upsert(vectors=[
    ("doc1", [0.1, 0.2, 0.3, ...], {"text": "Python basics"}),
    ("doc2", [0.2, 0.3, 0.4, ...], {"text": "Web development"}),
])

# Query
results = index.query(
    vector=[0.15, 0.25, 0.35, ...],  # Query embedding
    top_k=3,
    include_metadata=True
)

for match in results['matches']:
    print(f"{match['id']}: {match['score']:.4f}")
```

### Part 4: Integration Example (3 min)

**Creating a Search Interface**:

```python
def interactive_search():
    """Simple interactive search demo"""
    engine = SemanticSearchEngine()

    # Load documents (in real world, load from database)
    for doc_id, content in documents.items():
        engine.add_document(doc_id, content)

    engine.build_index()

    # Interactive loop
    print("\nEnter search queries (or 'quit' to exit):")
    while True:
        query = input("\nSearch: ").strip()

        if query.lower() == 'quit':
            break

        results = engine.search(query, top_k=3)

        print("\nResults:")
        for rank, (doc_id, score) in enumerate(results, 1):
            print(f"{rank}. {doc_id} (similarity: {score:.4f})")

# Uncomment to run interactive search
# interactive_search()
```

## =¡ Key Takeaways

- **Semantic search finds meaning**: Not just keyword matching
- **Embeddings enable fast searching**: Pre-compute once, search many times
- **Similarity scoring ranks results**: Highest similarity = most relevant
- **Scalability requires vector databases**: FAISS, Pinecone, Weaviate for production
- **Real-world applications**: Search, recommendation systems, content discovery
- **Ranking matters**: Return results in order of relevance
- **Cost optimization**: Use smaller embedding models for basic tasks

## = Resources

- [OpenAI Embeddings API](https://platform.openai.com/docs/guides/embeddings)
- [Pinecone Semantic Search Tutorial](https://docs.pinecone.io/)
- [Weaviate Semantic Search](https://weaviate.io/developers/weaviate)
- [FAISS for Similarity Search](https://github.com/facebookresearch/faiss)
- [Semantic Search Explained - Medium](https://towardsdatascience.com/semantic-search-5c6e6f9e55f)

##  Completion Checklist

- [ ] Understand the difference between keyword and semantic search
- [ ] Created and indexed documents with embeddings
- [ ] Implemented a working search system
- [ ] Tested with multiple queries
- [ ] Verified that similar documents rank higher
- [ ] Understand cosine similarity calculation
- [ ] Optional: Explored Pinecone or another vector database

## = Up Next

Tomorrow: [Day 17 - RAG - Retrieval Augmented Generation](./day17.md) - Learn how to combine search with AI generation for intelligent question answering!

---

**Progress**: Day 16 of 30
