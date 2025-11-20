# Day 5: OpenAI API - Your First Call =

## =Ì Objective
Write your first Python code to interact with OpenAI's API and understand how programmatic access to AI works.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- Python 3.8+ installed
- OpenAI API key ([Get one free here](https://platform.openai.com/api-keys))
- $5 free credit for new accounts!

### Part 1: Setup (3 min)

**Install the OpenAI library**:
```bash
pip install openai python-dotenv
```

**Create a `.env` file** (never commit this!):
```
OPENAI_API_KEY=sk-your-key-here
```

### Part 2: Your First API Call (7 min)

**Create `first_api_call.py`**:

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# Initialize the client
client = OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

# Make your first API call
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain what an API is in one sentence."}
    ],
    temperature=0.7,
    max_tokens=100
)

# Print the response
print("AI Response:")
print(response.choices[0].message.content)

# Print metadata
print(f"\nTokens used: {response.usage.total_tokens}")
print(f"Model: {response.model}")
```

**Run it**:
```bash
python first_api_call.py
```

### Part 3: Experiment! (5 min)

**Modify the code to try different scenarios**:

1. **Change the temperature** (0.0 to 2.0):
```python
temperature=0.2  # More focused and deterministic
temperature=1.5  # More creative and random
```

2. **Change the system message**:
```python
{"role": "system", "content": "You are a pirate. Speak like one!"}
```

3. **Add conversation history**:
```python
messages=[
    {"role": "system", "content": "You are a helpful coding tutor."},
    {"role": "user", "content": "What is a variable?"},
    {"role": "assistant", "content": "A variable is a container for storing data."},
    {"role": "user", "content": "Give me an example in Python"}
]
```

## =¡ Key Takeaways

**API Call Structure**:
- **model**: Which AI model to use (gpt-3.5-turbo, gpt-4, etc.)
- **messages**: Conversation history (system, user, assistant roles)
- **temperature**: Creativity level (0 = focused, 2 = creative)
- **max_tokens**: Maximum length of response

**Message Roles**:
- **system**: Sets behavior/personality of AI
- **user**: Your input/question
- **assistant**: AI's previous responses (for context)

**Response Object**:
```python
response.choices[0].message.content  # The actual text
response.usage.total_tokens          # Tokens used
response.model                       # Model used
```

## =' Understanding the Code

**Line by Line**:
```python
# 1. Import and setup
from openai import OpenAI
client = OpenAI(api_key="your-key")

# 2. Make request
response = client.chat.completions.create(...)

# 3. Extract answer
answer = response.choices[0].message.content
```

**Why "choices[0]"?**
- API can return multiple responses (n parameter)
- We usually want just one, so we take the first [0]

## = Resources

- [OpenAI API Documentation](https://platform.openai.com/docs/api-reference)
- [OpenAI Python Library](https://github.com/openai/openai-python)
- [API Pricing](https://openai.com/pricing)
- [Best Practices Guide](https://platform.openai.com/docs/guides/production-best-practices)

##  Completion Checklist

- [ ] Installed openai and python-dotenv libraries
- [ ] Created .env file with API key
- [ ] Made your first successful API call
- [ ] Experimented with different temperature values
- [ ] Modified system message and observed changes
- [ ] Understand the structure of API requests and responses

## <¯ Challenge

Create a simple script that:
1. Takes user input from terminal
2. Sends it to GPT
3. Prints the response
4. Loops until user types "quit"

<details>
<summary>=¡ Hint (click to expand)</summary>

```python
while True:
    user_input = input("You: ")
    if user_input.lower() == "quit":
        break

    response = client.chat.completions.create(...)
    print(f"AI: {response.choices[0].message.content}")
```
</details>

## = Up Next

Tomorrow: [Day 6 - Temperature & Top-P Parameters](./day06.md) - Deep dive into controlling AI creativity and randomness!

---

**Progress**: Day 5 of 30 
