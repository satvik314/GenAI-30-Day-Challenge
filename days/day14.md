# Day 14: Image to Text with CLIP

## =Ì Objective
Learn how CLIP (Contrastive Language-Image Pre-training) enables image understanding by connecting visual content with text, and implement practical image analysis using CLIP embeddings.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- Python 3.8+ installed
- PyTorch and CLIP library

**Setup** (2 min):
```bash
pip install torch torchvision transformers pillow
pip install git+https://github.com/openai/CLIP.git
```

### Part 1: Understanding CLIP (3 min)

**What is CLIP?**
- **CLIP** = Contrastive Language-Image Pre-training
- Understands the relationship between images and text descriptions
- Creates "embeddings" for both images and text in the same space
- Can classify images, find similar images, and answer visual questions

**How CLIP Works**:
```
1. Input an image and text descriptions
2. CLIP converts image to vector (embedding)
3. CLIP converts text to vector (embedding)
4. Compare similarity between vectors
5. Higher similarity = better match
```

**CLIP vs Other Vision Models**:
```
Traditional Image Classifier:
- Fixed classes (cat, dog, bird, etc.)
- Can't handle new categories
- Requires retraining for new classes

CLIP:
- Works with ANY text description
- Zero-shot learning (no retraining needed)
- More flexible and powerful
```

### Part 2: Image Classification with CLIP (7 min)

**Create `clip_classifier.py`**:

```python
import torch
import clip
from PIL import Image
import requests
from io import BytesIO

# Load CLIP model
device = "cuda" if torch.cuda.is_available() else "cpu"
model, preprocess = clip.load("ViT-B/32", device=device)

def classify_image(image_path, descriptions):
    """
    Classify an image using CLIP.

    Args:
        image_path: Path to image or URL
        descriptions: List of text descriptions to compare
    """

    # Load image
    if image_path.startswith('http'):
        response = requests.get(image_path)
        image = Image.open(BytesIO(response.content))
    else:
        image = Image.open(image_path)

    # Preprocess image
    image_tensor = preprocess(image).unsqueeze(0).to(device)

    # Tokenize text descriptions
    text_inputs = clip.tokenize(descriptions).to(device)

    # Get embeddings (no gradient needed)
    with torch.no_grad():
        image_features = model.encode_image(image_tensor)
        text_features = model.encode_text(text_inputs)

    # Normalize features
    image_features /= image_features.norm(dim=-1, keepdim=True)
    text_features /= text_features.norm(dim=-1, keepdim=True)

    # Calculate similarity
    similarity = (image_features @ text_features.T).softmax(dim=-1)

    # Results
    print(f"\nImage Analysis Results:")
    print("-" * 40)
    for i, description in enumerate(descriptions):
        score = similarity[0][i].item()
        print(f"{description:30s} {score:.4f}")

# Example usage
descriptions = [
    "a cat",
    "a dog",
    "a bird",
    "a car",
    "a landscape"
]

# Test with your own image
# classify_image("path/to/your/image.jpg", descriptions)

# Test with URL
test_image = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Cat03.jpg/1200px-Cat03.jpg"
classify_image(test_image, descriptions)
```

**Run it**:
```bash
python clip_classifier.py
```

### Part 3: Image Search with CLIP (5 min)

**Create `clip_search.py`** - Find similar images in a folder:

```python
import torch
import clip
from PIL import Image
import os
from pathlib import Path
import numpy as np

device = "cuda" if torch.cuda.is_available() else "cpu"
model, preprocess = clip.load("ViT-B/32", device=device)

def search_by_text(image_folder, query, top_k=5):
    """Find images most similar to a text query"""

    # Get all images
    image_files = list(Path(image_folder).glob("*.jpg")) + \
                  list(Path(image_folder).glob("*.png"))

    if not image_files:
        print("No images found in folder!")
        return

    # Encode query text
    query_tokens = clip.tokenize(query).to(device)
    with torch.no_grad():
        query_features = model.encode_text(query_tokens)
        query_features /= query_features.norm(dim=-1, keepdim=True)

    # Encode all images
    similarities = []
    for img_path in image_files:
        image = Image.open(img_path)
        image_tensor = preprocess(image).unsqueeze(0).to(device)

        with torch.no_grad():
            image_features = model.encode_image(image_tensor)
            image_features /= image_features.norm(dim=-1, keepdim=True)

        similarity = (query_features @ image_features.T).item()
        similarities.append((img_path.name, similarity))

    # Sort and display top results
    similarities.sort(key=lambda x: x[1], reverse=True)

    print(f"\nSearch Results for: '{query}'")
    print("-" * 40)
    for i, (filename, score) in enumerate(similarities[:top_k], 1):
        print(f"{i}. {filename:30s} (similarity: {score:.4f})")

# Usage
# search_by_text("./images", "a sunset over mountains")
search_by_text(".", "document with text", top_k=5)
```

### Part 4: Understanding CLIP Embeddings (3 min)

**Key Concepts**:

```python
# Image embedding (512-dimensional vector)
image_embedding = [0.234, -0.123, 0.567, ..., 0.891]

# Text embedding (same 512-dimensional space)
text_embedding = [0.245, -0.115, 0.571, ..., 0.889]

# Similarity = dot product of normalized embeddings
# Higher value = more similar
similarity = sum(image_embedding[i] * text_embedding[i])
```

## =¡ Key Takeaways

- **CLIP bridges vision and language**: Understands both images and text in the same embedding space
- **Zero-shot learning**: Classify images without training on specific categories
- **Flexible descriptions**: Use any text description, not just fixed labels
- **Embedding similarity**: Compare images and text by treating them as vectors
- **Computational efficiency**: CLIP is relatively lightweight compared to large vision models
- **Real applications**: Image search, content moderation, accessibility features

## = Resources

- [CLIP GitHub Repository](https://github.com/openai/CLIP)
- [CLIP Paper](https://arxiv.org/abs/2103.14030)
- [CLIP Tutorial - Hugging Face](https://huggingface.co/docs/transformers/model_doc/clip)
- [OpenAI CLIP Models](https://github.com/openai/CLIP/blob/main/model-card.md)

##  Completion Checklist

- [ ] Installed PyTorch, torchvision, and CLIP
- [ ] Successfully loaded a CLIP model
- [ ] Classified an image using multiple text descriptions
- [ ] Observed similarity scores between images and text
- [ ] Tested with different types of images (objects, scenes, documents)
- [ ] Understand how embeddings create a shared space for images and text

## = Up Next

Tomorrow: [Day 15 - Embeddings & Vector Databases](./day15.md) - Dive deeper into embeddings and learn how to store and search them efficiently!

---

**Progress**: Day 14 of 30
