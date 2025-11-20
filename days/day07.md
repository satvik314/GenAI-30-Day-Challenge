# Day 7: Prompt Templates with LangChain =

## =Ì Objective
Learn to create reusable prompt templates using LangChain to structure and scale your AI applications.

## <¯ Today's Exercise (10-15 minutes)

### What is LangChain?

**LangChain** is a framework for building AI applications that:
- Standardizes prompt structures
- Chains multiple AI calls together
- Integrates with various LLMs (OpenAI, Anthropic, etc.)
- Provides utilities for common tasks

### Part 1: Installation & Setup (2 min)

```bash
pip install langchain langchain-openai
```

### Part 2: Basic Prompt Templates (6 min)

**Simple Template** (`prompt_template_basic.py`):

```python
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate

load_dotenv()

# Initialize LLM
llm = ChatOpenAI(
    api_key=os.getenv('OPENAI_API_KEY'),
    model="gpt-3.5-turbo",
    temperature=0.7
)

# Create a prompt template
template = ChatPromptTemplate.from_messages([
    ("system", "You are a {profession} with {years} years of experience."),
    ("user", "{question}")
])

# Format the prompt
prompt = template.format_messages(
    profession="software engineer",
    years=10,
    question="What's the best way to learn Python?"
)

# Get response
response = llm.invoke(prompt)
print(response.content)
```

**Why templates?**
- Reusability: Write once, use many times
- Consistency: Same structure across calls
- Maintainability: Update in one place
- Type safety: Catch missing variables early

### Part 3: Multiple Template Patterns (7 min)

**Pattern 1: Product Description Generator**

```python
from langchain.prompts import PromptTemplate

product_template = PromptTemplate(
    input_variables=["product", "features", "audience"],
    template="""Create a compelling product description for:

Product: {product}
Key Features: {features}
Target Audience: {audience}

Write a 50-word description that highlights benefits."""
)

# Use it
prompt = product_template.format(
    product="Smart Water Bottle",
    features="Hydration tracking, LED reminders, App integration",
    audience="Health-conscious professionals"
)

response = llm.invoke(prompt)
print(response)
```

**Pattern 2: Email Generator**

```python
email_template = PromptTemplate(
    input_variables=["recipient_type", "purpose", "tone"],
    template="""Write an email to a {recipient_type}.
Purpose: {purpose}
Tone: {tone}

Include subject line and body."""
)

# Generate multiple emails
scenarios = [
    {"recipient_type": "client", "purpose": "project update", "tone": "professional"},
    {"recipient_type": "team member", "purpose": "meeting reminder", "tone": "friendly"},
]

for scenario in scenarios:
    prompt = email_template.format(**scenario)
    response = llm.invoke(prompt)
    print(f"\n{'='*60}")
    print(response)
```

**Pattern 3: Few-Shot Template**

```python
from langchain.prompts.few_shot import FewShotPromptTemplate
from langchain.prompts import PromptTemplate

# Examples for the AI to learn from
examples = [
    {"word": "happy", "antonym": "sad"},
    {"word": "hot", "antonym": "cold"},
    {"word": "fast", "antonym": "slow"},
]

# Format for each example
example_template = PromptTemplate(
    input_variables=["word", "antonym"],
    template="Word: {word}\nAntonym: {antonym}"
)

# Create few-shot template
few_shot_template = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_template,
    prefix="Give the antonym of each word:",
    suffix="Word: {input}\nAntonym:",
    input_variables=["input"]
)

# Use it
prompt = few_shot_template.format(input="big")
response = llm.invoke(prompt)
print(response)
```

## =¡ Key Takeaways

**Benefits of Templates**:
- **DRY Principle**: Don't Repeat Yourself
- **Scalability**: Easy to create variations
- **Testing**: Consistent structure for testing
- **Collaboration**: Shareable template library

**LangChain Prompt Types**:
1. **PromptTemplate**: Simple string substitution
2. **ChatPromptTemplate**: For chat models (system, user, assistant)
3. **FewShotPromptTemplate**: Include examples
4. **PipelinePromptTemplate**: Combine multiple templates

**Best Practices**:
```python
#  Good: Clear variable names
template = "Explain {concept} to a {audience}"

# L Bad: Vague variables
template = "Explain {thing} to {people}"

#  Good: Provide defaults
template = PromptTemplate(
    input_variables=["topic"],
    template="Explain {topic} in simple terms"
)

#  Good: Validate inputs
template.format(topic="AI")  # Will error if missing
```

## =' Real-World Application

Create a **content generator** for social media:

```python
social_template = ChatPromptTemplate.from_messages([
    ("system", "You are a {platform} content expert."),
    ("user", """Create a {platform} post about {topic}.

Requirements:
- Tone: {tone}
- Include {num_hashtags} relevant hashtags
- Length: Suitable for {platform}
- Call-to-action: {cta}""")
])

# Generate for different platforms
platforms = [
    {"platform": "Twitter", "topic": "AI productivity", "tone": "casual",
     "num_hashtags": 3, "cta": "Follow for more tips"},
    {"platform": "LinkedIn", "topic": "AI productivity", "tone": "professional",
     "num_hashtags": 2, "cta": "Share your thoughts"},
]

for config in platforms:
    prompt = social_template.format_messages(**config)
    response = llm.invoke(prompt)
    print(f"\n{config['platform']} Post:")
    print(response.content)
```

## = Resources

- [LangChain Documentation](https://python.langchain.com/docs/get_started/introduction)
- [Prompt Templates Guide](https://python.langchain.com/docs/modules/model_io/prompts/prompt_templates/)
- [LangChain Cookbook](https://github.com/langchain-ai/langchain/tree/master/cookbook)

##  Completion Checklist

- [ ] Installed LangChain and langchain-openai
- [ ] Created your first PromptTemplate
- [ ] Built a ChatPromptTemplate with system and user messages
- [ ] Experimented with few-shot prompting
- [ ] Created at least one template for a real-world use case
- [ ] Understand when to use templates vs direct prompts

## <¯ Challenge

Create a **"Code Review Bot"** template that:
1. Takes code snippet as input
2. Specifies programming language
3. Sets review depth (quick/thorough)
4. Returns structured feedback (bugs, style, improvements)

Try it with different languages and code samples!

## = Up Next

Tomorrow: [Day 8 - DALL-E: Text to Image](./day08.md) - Start creating images with AI!

---

**Progress**: Day 7 of 30 
