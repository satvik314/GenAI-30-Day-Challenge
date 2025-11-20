# Day 15: Embeddings & Vector Databases

## =Ì Objective
Understand what embeddings are, how they represent meaning in AI systems, and explore vector databases for efficient similarity search and information retrieval.

## <¯ Today's Exercise (10-15 minutes)

### Part 1: What Are Embeddings? (4 min)

**Understanding Embeddings**:
- **Embedding**: A numerical representation of text, images, or other data
- Converts words, sentences, or documents into vectors (lists of numbers)
- Captures semantic meaning - similar content gets similar embeddings

**Example**:

```
Text: "The cat sat on the mat"
Embedding: [0.234, -0.123, 0.567, ..., 0.891]
         (512-dimensional vector for GPT embeddings)

Text: "A feline rested on the rug"
Embedding: [0.241, -0.119, 0.572, ..., 0.885]
         (Very similar to above, high similarity score)

Text: "The car drove quickly"
Embedding: [0.091, 0.456, -0.234, ..., 0.123]
         (Very different, low similarity score)
```

**How Embeddings Capture Meaning**:

```
Vector Space Example (simplified 2D):

           high tech
                |
                |
   [Tesla]      |   [Robot]
        \       |      /
         \      |     /
          \     |    /
    -------+-----+------
           |     |
      [Cat]      |   [Car]
                 |
            pets/animals
```

**Key Properties**:
- **Dimensionality**: 384, 512, 768, 1536+ dimensions (not 2D!)
- **Similarity**: Close vectors = related meaning
- **Distance**: Measured using cosine similarity, L2 distance, or dot product
- **Composable**: Can be averaged, combined, and manipulated

### Part 2: Creating Embeddings with OpenAI (4 min)

**Setup** (1 min):
```bash
pip install openai numpy scikit-learn python-dotenv
```

**Create `embeddings_demo.py`**:

```python
import os
from openai import OpenAI
from dotenv import load_dotenv
import numpy as np

load_dotenv()

client = OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

def get_embedding(text):
    """Get embedding for a text string"""
    response = client.embeddings.create(
        model="text-embedding-3-small",  # or text-embedding-3-large
        input=text
    )
    return response.data[0].embedding

def cosine_similarity(vec1, vec2):
    """Calculate cosine similarity between two vectors"""
    return np.dot(vec1, vec2) / (np.linalg.norm(vec1) * np.linalg.norm(vec2))

# Create embeddings for different sentences
sentences = [
    "The quick brown fox jumps over the lazy dog",
    "A fast orange fox leaps over a sleeping dog",
    "The cat climbed the tall tree",
    "Paris is the capital of France",
    "The Eiffel Tower is in Paris",
    "Artificial intelligence is transforming technology"
]

print("Generating embeddings...")
embeddings = {}
for sentence in sentences:
    embedding = get_embedding(sentence)
    embeddings[sentence] = embedding
    print(f" Embedded: {sentence[:50]}...")

# Compare similarities
print("\n" + "="*60)
print("SIMILARITY ANALYSIS")
print("="*60)

# Compare first three sentences (should be similar)
sim_1_2 = cosine_similarity(
    embeddings[sentences[0]],
    embeddings[sentences[1]]
)
print(f"\nSentence 1: {sentences[0][:40]}...")
print(f"Sentence 2: {sentences[1][:40]}...")
print(f"Similarity: {sim_1_2:.4f} (High - both about fox and dog)")

# Compare sentence 1 and 3 (should be less similar)
sim_1_3 = cosine_similarity(
    embeddings[sentences[0]],
    embeddings[sentences[2]]
)
print(f"\nSentence 1: {sentences[0][:40]}...")
print(f"Sentence 3: {sentences[2][:40]}...")
print(f"Similarity: {sim_1_3:.4f} (Lower - different animals)")

# Compare sentences about Paris
sim_paris = cosine_similarity(
    embeddings[sentences[3]],
    embeddings[sentences[4]]
)
print(f"\nSentence 4: {sentences[3][:40]}...")
print(f"Sentence 5: {sentences[4][:40]}...")
print(f"Similarity: {sim_paris:.4f} (Very high - both about Paris)")

# Information about embeddings
print(f"\n{'='*60}")
print(f"Embedding Dimensions: {len(embeddings[sentences[0]])}")
print(f"Tokens processed (approx): {len(sentences) * 20}")
```

**Run it**:
```bash
python embeddings_demo.py
```

### Part 3: Introduction to Vector Databases (4 min)

**What is a Vector Database?**
- Database optimized for storing and searching embeddings
- Traditional databases: Search by exact match or keywords
- Vector databases: Search by semantic similarity

**Popular Vector Databases**:
```
1. Pinecone - Cloud-based, fully managed
2. Weaviate - Open-source, flexible
3. Milvus - High-performance, open-source
4. FAISS - Facebook's similarity search library
5. Qdrant - Modern vector database
6. Chroma - Lightweight, great for local use
```

**Why Use Vector Databases?**
- Instant similarity search on millions of embeddings
- Scalable semantic search
- Better than traditional keyword search
- Foundation for RAG systems

**Simple Vector Search Example**:

```python
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

# Imagine these are your stored document embeddings
documents = {
    "doc1": "Embeddings are numerical representations",
    "doc2": "Vector databases store embeddings efficiently",
    "doc3": "The weather is sunny today",
}

# In reality, these would be embeddings
embeddings = {
    "doc1": np.random.rand(1536),  # text-embedding-3-large = 3072 dims
    "doc2": np.random.rand(1536),
    "doc3": np.random.rand(1536),
}

# Query embedding
query = "How do vector databases work?"
query_embedding = np.random.rand(1536)

# Search
results = []
for doc_id, doc_embedding in embeddings.items():
    similarity = cosine_similarity(
        [query_embedding],
        [doc_embedding]
    )[0][0]
    results.append((doc_id, similarity))

# Sort by similarity
results.sort(key=lambda x: x[1], reverse=True)

print("Top Results for Query:")
for doc_id, similarity in results[:3]:
    print(f"{doc_id}: {similarity:.4f}")
```

## =¡ Key Takeaways

- **Embeddings convert meaning to numbers**: Text becomes high-dimensional vectors
- **Similarity = semantic relationship**: Cosine similarity measures how related texts are
- **Vector databases are specialized**: Optimized for fast similarity search
- **Efficiency matters at scale**: With millions of documents, traditional search fails
- **Embeddings are model-dependent**: Different models produce different embeddings
- **Dimensions affect quality**: Larger embeddings capture more nuance
- **Cost consideration**: Larger models = more expensive embeddings

## = Resources

- [OpenAI Embeddings API](https://platform.openai.com/docs/guides/embeddings)
- [Understanding Embeddings - Hugging Face](https://huggingface.co/docs/sentence-transformers/)
- [Vector Database Comparison](https://www.sicara.ai/blog/vector-databases-comparison)
- [Pinecone Documentation](https://docs.pinecone.io/)
- [Weaviate Introduction](https://weaviate.io/developers/weaviate)
- [FAISS Tutorial](https://github.com/facebookresearch/faiss/wiki)

##  Completion Checklist

- [ ] Understand what embeddings are and why they matter
- [ ] Know the difference between text and embeddings
- [ ] Successfully created embeddings using OpenAI API
- [ ] Calculated cosine similarity between embeddings
- [ ] Observed that similar texts have similar embeddings
- [ ] Learned about vector database options
- [ ] Understand why vector databases are needed at scale

## = Up Next

Tomorrow: [Day 16 - Semantic Search Implementation](./day16.md) - Build a real semantic search system using embeddings and vector databases!

---

**Progress**: Day 15 of 30
