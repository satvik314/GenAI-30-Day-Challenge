# Day 26: Ollama - Local LLMs

## =Ì Objective
Learn how to run large language models locally on your machine using Ollama, enabling privacy-first AI development without relying on cloud APIs.

## <¯ Today's Exercise (10-15 minutes)

### Part 1: Understanding Ollama (3 min)

**What is Ollama?**

Ollama is a framework that:
- Runs large language models locally on your machine
- Requires no internet connection after download
- Works on macOS, Linux, and Windows
- Supports dozens of models (Llama, Mistral, Neural Chat, etc.)
- Provides a simple command-line interface
- Exposes a local API (localhost:11434)

**Why Run Models Locally?**

- **Privacy**: Your data never leaves your machine
- **Cost**: No API subscription fees
- **Speed**: Instant inference (no network latency)
- **Customization**: Full control over the model
- **Offline**: Works without internet connection
- **Experimentation**: Easy to try different models

**Available Models** (via Ollama):

1. **Llama 2**: 7B, 13B, 70B (Meta's open-source)
2. **Mistral**: 7B (fast, efficient)
3. **Neural Chat**: 7B (optimized for chat)
4. **Orca 2**: 7B, 13B (instruction following)
5. **Dolphin Mixtral**: 8x7B (mixture of experts)
6. **And many more...**

**Model Sizes & Performance**:

| Model | Size | Speed | Quality | Memory |
|-------|------|-------|---------|--------|
| Mistral 7B | 4.1GB | Fast | Good | 8GB |
| Llama 2 7B | 3.8GB | Fast | Good | 8GB |
| Llama 2 13B | 7.3GB | Medium | Very Good | 16GB |
| Mistral 8x7B | 26GB | Slow | Excellent | 32GB |

### Part 2: Getting Started with Ollama (4 min)

**Installation**:

```bash
# macOS:
# Download from https://ollama.ai
# Or: brew install ollama

# Linux:
curl https://ollama.ai/install.sh | sh

# Windows:
# Download installer from https://ollama.ai

# Start Ollama service:
ollama serve
```

**Pull a Model**:

```bash
# Pull Mistral (small, fast)
ollama pull mistral

# Pull Llama 2
ollama pull llama2

# Pull Neural Chat
ollama pull neural-chat

# List downloaded models
ollama list
```

**Run a Model**:

```bash
# Interactive chat
ollama run mistral

# Or in terminal:
echo "What is machine learning?" | ollama run mistral
```

**Using Ollama API**:

```bash
# Start server (if not already running):
ollama serve

# In another terminal, test the API:
curl http://localhost:11434/api/generate \
  -d '{
    "model": "mistral",
    "prompt": "What is AI?",
    "stream": false
  }'
```

### Part 3: Building Applications with Ollama (5 min)

**Create `ollama_demo.py`**:

```python
import requests
import json

# Ollama API endpoint
OLLAMA_API = "http://localhost:11434/api/generate"

print("="*60)
print("OLLAMA LOCAL LLM EXAMPLES")
print("="*60)

# Example 1: Simple completion
print("\nExample 1: Simple Completion")
print("-" * 40)

prompt = "Explain quantum computing in 2 sentences"
response = requests.post(OLLAMA_API, json={
    "model": "mistral",
    "prompt": prompt,
    "stream": False
})

result = response.json()
print(f"Prompt: {prompt}")
print(f"Response: {result['response']}\n")

# Example 2: Streaming response
print("Example 2: Streaming Response")
print("-" * 40)

prompt = "Write a haiku about AI"
print(f"Prompt: {prompt}\n")

response = requests.post(OLLAMA_API, json={
    "model": "mistral",
    "prompt": prompt,
    "stream": True
})

print("Response (streaming):")
for line in response.iter_lines():
    if line:
        chunk = json.loads(line)
        print(chunk['response'], end='', flush=True)
print("\n")

# Example 3: Chat-style conversation
print("\nExample 3: Multi-turn Conversation")
print("-" * 40)

def chat_with_ollama(model, conversation_history):
    """Send conversation to Ollama"""
    prompt = "\n".join(conversation_history)

    response = requests.post(OLLAMA_API, json={
        "model": model,
        "prompt": prompt,
        "stream": False
    })

    return response.json()['response']

# Simulate a conversation
conversation = [
    "User: What is the capital of France?",
    "Assistant: The capital of France is Paris.",
    "User: How many people live there?"
]

response = chat_with_ollama("mistral", conversation)
print(f"Context: {conversation}")
print(f"Response: {response}\n")

# Example 4: Using Langchain with Ollama
print("="*60)
print("LANGCHAIN WITH OLLAMA INTEGRATION")
print("-" * 40)

langchain_example = """
from langchain.llms import Ollama
from langchain.callbacks.manager import CallbackManager
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler

# Create Ollama LLM
llm = Ollama(
    model="mistral",
    callback_managers=[CallbackManager([StreamingStdOutCallbackHandler()])],
)

# Use it like any other LangChain LLM
result = llm("What is machine learning?")
print(result)

# Build chains
from langchain.prompts import PromptTemplate

template = "Question: {question}\\n\\nAnswer:"
prompt = PromptTemplate(template=template, input_variables=["question"])
chain = prompt | llm

response = chain.invoke({"question": "What is AI?"})
print(response)
"""

print(langchain_example)

# Example 5: Performance comparison
print("\n" + "="*60)
print("MODEL PERFORMANCE COMPARISON")
print("="*60)

models_to_test = ["mistral", "neural-chat"]
test_prompt = "Briefly explain what is deep learning"

print(f"Test Prompt: {test_prompt}\n")

for model in models_to_test:
    print(f"Testing {model}...")
    try:
        response = requests.post(OLLAMA_API, json={
            "model": model,
            "prompt": test_prompt,
            "stream": False
        })
        result = response.json()
        print(f"  Response: {result['response'][:100]}...")
    except Exception as e:
        print(f"  Error: {e}")
    print()

print("="*60)
print("OLLAMA BEST PRACTICES")
print("="*60)

best_practices = """
1. CHOOSING MODELS
   - Start with Mistral 7B (fastest, good quality)
   - For better quality: Llama 2 13B
   - For complex tasks: Orca 2 or Dolphin

2. MEMORY MANAGEMENT
   - Monitor RAM usage while running models
   - Unload models after use
   - Run smaller models on limited hardware

3. PROMPTING STRATEGIES
   - Be specific in your prompts
   - Use few-shot examples
   - Iterate on prompt engineering

4. INTEGRATION
   - Use Ollama API in your applications
   - Combine with LangChain
   - Build RAG systems locally

5. PERFORMANCE
   - First response may be slow (model loading)
   - Subsequent requests are faster
   - GPU acceleration available on supported hardware

6. PRIVACY & SECURITY
   - No data leaves your machine
   - Can run sensitive/private data locally
   - No subscription or authentication needed
"""

print(best_practices)
```

**Run it** (assuming Ollama is running):

```bash
pip install requests
python ollama_demo.py
```

## =¡ Key Takeaways

- **Local LLMs are practical now** - consumer hardware can run capable models
- **Ollama makes it easy** - simple CLI and API
- **Privacy-first** - your data never leaves your machine
- **No API costs** - one-time download, unlimited inference
- **Offline capable** - works without internet connection
- **Great for development** - fast iteration on prompts
- **Integrates well** - works with LangChain, LlamaIndex, etc.
- **Model selection matters** - choose based on your hardware and needs

**Local vs Cloud Trade-offs**:

| Aspect | Local (Ollama) | Cloud (API) |
|--------|---|---|
| **Privacy** | Excellent | Limited |
| **Cost** | Low | High (per token) |
| **Speed** | Medium | Fast |
| **Hardware Required** | 8GB+ RAM | None |
| **Latest Models** | Slower updates | Immediate |
| **Offline** | Yes | No |

## = Resources

- [Ollama Official Site](https://ollama.ai)
- [Ollama GitHub](https://github.com/jmorganca/ollama)
- [Available Models](https://ollama.ai/library)
- [Ollama with LangChain](https://python.langchain.com/docs/integrations/llms/ollama)
- [LlamaIndex Integration](https://docs.llamaindex.ai/en/stable/module_guides/models/llms/integrations/ollama/)

##  Completion Checklist

- [ ] Installed Ollama on your machine
- [ ] Downloaded at least one model (Mistral recommended)
- [ ] Ran a model interactively using CLI
- [ ] Used the Ollama API with Python
- [ ] Tested streaming responses
- [ ] Built a simple application with Ollama
- [ ] Integrated Ollama with LangChain
- [ ] Compared different models
- [ ] Understand privacy benefits of local LLMs
- [ ] Know when to use Ollama vs cloud APIs

## =­ Challenge

Build a local AI application using Ollama:
1. Create a question-answering system for a specific domain
2. Combine Ollama with RAG (Retrieval-Augmented Generation)
3. Create a chatbot that remembers conversation context
4. Experiment with different model sizes and compare quality vs speed

## = Up Next

Tomorrow: [Day 27 - LangChain Agents](./day27.md) - Build autonomous AI agents that can use tools and take actions!

---

**Progress**: Day 26 of 30

**Week 4 Closing**: You've mastered TTS, Whisper, fine-tuning, and local models. Impressive progress!
