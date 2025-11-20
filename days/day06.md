# Day 6: Temperature & Top-P Parameters <!

## =Ì Objective
Master the temperature and top_p parameters to control AI creativity, randomness, and output consistency.

## <¯ Today's Exercise (10-15 minutes)

### Understanding Temperature

**Temperature** controls randomness in AI responses:
- **0.0**: Deterministic, focused, consistent
- **1.0**: Balanced (default)
- **2.0**: Creative, random, unpredictable

**Think of it like cooking**:
- Low temp: Following a recipe exactly
- High temp: Improvising with ingredients

### Part 1: Temperature Experiments (8 min)

**Test Script** (`temperature_test.py`):

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
client = OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

prompt = "Write a creative opening line for a sci-fi novel."
temperatures = [0.0, 0.7, 1.5]

for temp in temperatures:
    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        temperature=temp,
        max_tokens=50
    )

    print(f"\n{'='*60}")
    print(f"Temperature: {temp}")
    print(f"{'='*60}")
    print(response.choices[0].message.content)
```

**Run it 3 times** and observe:
- Does temperature=0.0 give the same result each time?
- How different are the temperature=1.5 results?
- Which temperature gives the most creative output?

### Part 2: Top-P (Nucleus Sampling) (4 min)

**Top-P** is an alternative to temperature:
- Considers only the top P% of probable tokens
- **0.1**: Very focused (top 10% of options)
- **0.9**: More diverse (top 90% of options)

**Compare Temperature vs Top-P**:

```python
# Test 1: Low temperature
response1 = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Name a color"}],
    temperature=0.2
)

# Test 2: Low top_p
response2 = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Name a color"}],
    temperature=1.0,
    top_p=0.1
)

print(f"Low temp: {response1.choices[0].message.content}")
print(f"Low top_p: {response2.choices[0].message.content}")
```

**Note**: Don't use temperature and top_p together! Pick one.

### Part 3: Real-World Use Cases (3 min)

**When to use different settings**:

```python
# Use Case 1: Code generation (need consistency)
temperature = 0.2
# "Write a Python function to sort a list"

# Use Case 2: Creative writing (need variety)
temperature = 1.3
# "Write a short fantasy story"

# Use Case 3: Data extraction (need accuracy)
temperature = 0.0
# "Extract the email address from this text"

# Use Case 4: Brainstorming (need diversity)
temperature = 1.8
# "Give me 10 unique business name ideas"
```

## =¡ Key Takeaways

**Temperature Guide**:
- **0.0 - 0.3**: Factual, consistent, deterministic
  - Best for: Code, data extraction, formatting, translations
- **0.4 - 0.7**: Balanced
  - Best for: General Q&A, explanations, summaries
- **0.8 - 1.5**: Creative, diverse
  - Best for: Writing, brainstorming, creative tasks
- **1.6 - 2.0**: Very random (rarely needed)
  - Best for: Experimental creative tasks

**Top-P Guide**:
- **0.1 - 0.4**: Focused, consistent
- **0.5 - 0.7**: Balanced
- **0.8 - 1.0**: Diverse, exploratory

**Pro Tips**:
- Use **temperature** for most cases (more intuitive)
- Use **top_p** when you need fine control over diversity
- Never adjust both simultaneously
- For production: Start conservative (temp=0.3), increase if needed

## =' Interactive Experiment

Try this comparison yourself:

```python
tasks = [
    ("Explain quantum physics", 0.2),  # Factual
    ("Write a haiku about coffee", 1.2),  # Creative
    ("Extract date from: 'Meeting on Jan 15'", 0.0),  # Precise
]

for task, temp in tasks:
    # Run the task with suggested temperature
    # Compare with different temperatures
    # Which works best?
```

## = Resources

- [OpenAI Temperature Guide](https://platform.openai.com/docs/api-reference/chat/create#temperature)
- [Understanding Sampling Methods](https://huggingface.co/blog/how-to-generate)
- [Temperature vs Top-P](https://docs.cohere.com/docs/temperature)

##  Completion Checklist

- [ ] Ran the temperature experiment and observed differences
- [ ] Tested the same prompt with 3+ different temperatures
- [ ] Understand when to use low vs high temperature
- [ ] Tried top_p and compared to temperature
- [ ] Know which parameter to use for common tasks (code, creative writing, etc.)
- [ ] Created a personal reference guide for temperature settings

## <¯ Challenge

Create a script that generates:
1. **5 company names** with temp=1.5 (creative)
2. **A tagline** for each with temp=0.7 (balanced)
3. **A formal description** with temp=0.3 (professional)

See how temperature affects each type of content!

## = Up Next

Tomorrow: [Day 7 - Prompt Templates with LangChain](./day07.md) - Learn to structure and reuse prompts efficiently!

---

**Progress**: Day 6 of 30 
