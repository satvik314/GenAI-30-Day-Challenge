# Day 20: Chaining Prompts

## =Ì Objective
Master sequential AI operations by chaining multiple LLM calls together to solve complex tasks that require multi-step reasoning, transformation, and refinement.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- Python 3.8+
- `pip install langchain langchain-openai python-dotenv`
- OpenAI API key

### Part 1: Introduction to Prompt Chaining (2 min)

**What is Prompt Chaining?**

Simple tasks can be solved in one prompt, but complex tasks need multiple steps:

```
SINGLE PROMPT APPROACH:
Input ’ LLM ’ Output
Problem: Complex tasks lose accuracy in one shot

CHAINING APPROACH:
Input ’ LLM ’ Output (Step 1)
         “
      Output ’ LLM‚ ’ Output‚ (Step 2)
         “
      Output‚ ’ LLMƒ ’ Outputƒ (Step 3)
         “
      Final Output
```

**Real Examples**:

```
1. Article Generation Chain:
   Topic ’ Outline Generator ’ Section Writer ’ Editor ’ Published Article

2. Code Review Chain:
   Code ’ Syntax Checker ’ Logic Analyzer ’ Optimization Suggester ’ Report

3. Translation Chain:
   English ’ Translation to Spanish ’ Translation to French ’ Quality Check

4. Customer Support Chain:
   Ticket ’ Categorizer ’ Solution Finder ’ Response Generator ’ Quality Checker
```

### Part 2: Simple Chain Example (4 min)

**Create `prompt_chaining.py`**:

```python
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

load_dotenv()

# Initialize LLM
llm = ChatOpenAI(
    model="gpt-3.5-turbo",
    api_key=os.getenv("OPENAI_API_KEY"),
    temperature=0.7
)

# ============================================================
# EXAMPLE 1: CONTENT GENERATION CHAIN
# ============================================================

print("="*60)
print("PROMPT CHAINING EXAMPLES")
print("="*60)

print("\nEXAMPLE 1: BLOG POST GENERATION")
print("-" * 40)

# Step 1: Generate outline
outline_prompt = ChatPromptTemplate.from_template(
    """Create a blog post outline for the topic: {topic}

    Format as a numbered list with main sections and 2-3 subsections each.

    Outline:"""
)

# Step 2: Generate intro
intro_prompt = ChatPromptTemplate.from_template(
    """Write an engaging introduction for a blog post with this outline:

    {outline}

    The introduction should be 2-3 sentences and hook the reader.

    Introduction:"""
)

# Step 3: Generate main content
content_prompt = ChatPromptTemplate.from_template(
    """Write the main body of a blog post based on this outline and introduction:

    Outline: {outline}
    Introduction: {introduction}

    Write 2-3 detailed paragraphs covering the main sections.

    Main Content:"""
)

# Chain them together
outline_chain = outline_prompt | llm | StrOutputParser()
intro_chain = intro_prompt | llm | StrOutputParser()
content_chain = content_prompt | llm | StrOutputParser()

topic = "Introduction to Machine Learning"

# Execute chain
print(f"Topic: {topic}\n")

print("Step 1: Generating outline...")
outline = outline_chain.invoke({"topic": topic})
print(f"Outline:\n{outline}\n")

print("Step 2: Generating introduction...")
intro = intro_chain.invoke({"outline": outline})
print(f"Introduction:\n{intro}\n")

print("Step 3: Generating main content...")
content = content_chain.invoke({
    "outline": outline,
    "introduction": intro
})
print(f"Content:\n{content[:300]}...\n")

# ============================================================
# EXAMPLE 2: CODE REVIEW CHAIN
# ============================================================

print("\n" + "="*60)
print("EXAMPLE 2: CODE REVIEW CHAIN")
print("-" * 40)

# Sample code to review
sample_code = """
def calculate_average(numbers):
    sum = 0
    for i in range(len(numbers)):
        sum += numbers[i]
    average = sum / len(numbers)
    return average
"""

# Step 1: Check syntax
syntax_prompt = ChatPromptTemplate.from_template(
    """Review this Python code for syntax errors and issues:

    {code}

    List any syntax problems found.

    Syntax Review:"""
)

# Step 2: Suggest improvements
improvement_prompt = ChatPromptTemplate.from_template(
    """Given this code and syntax review, suggest improvements for readability and performance:

    Code: {code}
    Syntax Review: {syntax_review}

    Improvements:"""
)

# Step 3: Provide refactored version
refactor_prompt = ChatPromptTemplate.from_template(
    """Based on the improvements suggested, provide a refactored version:

    Original Code: {code}
    Improvements: {improvements}

    Refactored Code:"""
)

syntax_chain = syntax_prompt | llm | StrOutputParser()
improvement_chain = improvement_prompt | llm | StrOutputParser()
refactor_chain = refactor_prompt | llm | StrOutputParser()

print(f"Code to Review:\n{sample_code}\n")

print("Step 1: Syntax review...")
syntax_review = syntax_chain.invoke({"code": sample_code})
print(f"Review: {syntax_review[:200]}...\n")

print("Step 2: Improvement suggestions...")
improvements = improvement_chain.invoke({
    "code": sample_code,
    "syntax_review": syntax_review
})
print(f"Improvements: {improvements[:200]}...\n")

print("Step 3: Generating refactored code...")
refactored = refactor_chain.invoke({
    "code": sample_code,
    "improvements": improvements
})
print(f"Refactored:\n{refactored[:300]}...\n")

# ============================================================
# EXAMPLE 3: TRANSLATION CHAIN
# ============================================================

print("="*60)
print("EXAMPLE 3: TRANSLATION CHAIN")
print("-" * 40)

text = "The quick brown fox jumps over the lazy dog"

# Step 1: Translate to Spanish
spanish_prompt = ChatPromptTemplate.from_template(
    """Translate this English text to Spanish:

    {text}

    Spanish translation:"""
)

# Step 2: Translate Spanish to French
french_prompt = ChatPromptTemplate.from_template(
    """Translate this Spanish text to French:

    {spanish_text}

    French translation:"""
)

# Step 3: Back-translate to English (verification)
english_prompt = ChatPromptTemplate.from_template(
    """Translate this French text back to English:

    {french_text}

    English translation:"""
)

spanish_chain = spanish_prompt | llm | StrOutputParser()
french_chain = french_prompt | llm | StrOutputParser()
english_chain = english_prompt | llm | StrOutputParser()

print(f"Original (English): {text}\n")

spanish = spanish_chain.invoke({"text": text})
print(f"Spanish: {spanish}\n")

french = french_chain.invoke({"spanish_text": spanish})
print(f"French: {french}\n")

english = english_chain.invoke({"french_text": french})
print(f"Back to English: {english}\n")

# ============================================================
# EXAMPLE 4: CUSTOMER SUPPORT CHAIN
# ============================================================

print("="*60)
print("EXAMPLE 4: CUSTOMER SUPPORT CHAIN")
print("-" * 40)

support_ticket = "My laptop won't turn on. The power button doesn't respond."

# Step 1: Categorize
categorize_prompt = ChatPromptTemplate.from_template(
    """Categorize this support ticket:

    {ticket}

    Provide: Category and Priority (high/medium/low)

    Categorization:"""
)

# Step 2: Generate troubleshooting steps
troubleshoot_prompt = ChatPromptTemplate.from_template(
    """Based on this support ticket, generate troubleshooting steps:

    Ticket: {ticket}
    Category: {category}

    Provide 3-5 step-by-step troubleshooting steps.

    Troubleshooting Steps:"""
)

# Step 3: Generate response
response_prompt = ChatPromptTemplate.from_template(
    """Write a professional customer support response:

    Ticket: {ticket}
    Troubleshooting: {troubleshooting}

    Response:"""
)

categorize_chain = categorize_prompt | llm | StrOutputParser()
troubleshoot_chain = troubleshoot_prompt | llm | StrOutputParser()
response_chain = response_prompt | llm | StrOutputParser()

print(f"Ticket: {support_ticket}\n")

print("Step 1: Categorizing...")
category = categorize_chain.invoke({"ticket": support_ticket})
print(f"Category: {category}\n")

print("Step 2: Generating troubleshooting...")
troubleshooting = troubleshoot_chain.invoke({
    "ticket": support_ticket,
    "category": category
})
print(f"Troubleshooting:\n{troubleshooting[:250]}...\n")

print("Step 3: Generating response...")
response = response_chain.invoke({
    "ticket": support_ticket,
    "troubleshooting": troubleshooting
})
print(f"Response:\n{response[:300]}...\n")

# ============================================================
# BEST PRACTICES
# ============================================================

print("="*60)
print("BEST PRACTICES FOR PROMPT CHAINING")
print("="*60)

best_practices = """
1. CLARITY IN EACH STEP
   - Make each step's objective clear
   - Include format specifications
   - Provide examples if complex

2. ERROR HANDLING
   - Validate outputs at each step
   - Handle edge cases
   - Have fallback strategies

3. EFFICIENCY
   - Avoid redundant information
   - Only pass necessary context
   - Consider parallel processing for independent steps

4. QUALITY CONTROL
   - Add validation steps
   - Cross-check important information
   - Include review/refinement steps

5. COST OPTIMIZATION
   - Use cheaper models for simple steps
   - Cache repeated computations
   - Combine steps when possible

6. TRANSPARENCY
   - Log each step's output
   - Include reasoning information
   - Track decision points
"""

print(best_practices)
```

**Run it**:
```bash
python prompt_chaining.py
```

### Part 3: Advanced Chaining Patterns (3 min)

**Pattern 1: Conditional Chaining**:

```python
# Route based on intermediate result
route_prompt = ChatPromptTemplate.from_template(
    "Categorize this: {input}")

categorizer = route_prompt | llm

# Based on category, use different chain
tech_chain = ...  # For technical issues
billing_chain = ... # For billing issues

# Conditional logic
def route_chain(input_text):
    category = categorizer.invoke({"input": input_text})
    if "technical" in category.lower():
        return tech_chain.invoke(input_text)
    else:
        return billing_chain.invoke(input_text)
```

**Pattern 2: Parallel Chaining**:

```python
from langchain_core.runnables import RunnableParallel

# Run multiple analyses in parallel
analysis_chain = RunnableParallel({
    "sentiment": sentiment_analyzer,
    "toxicity": toxicity_detector,
    "topics": topic_extractor
})

results = analysis_chain.invoke(text)
# Returns dict with all three results
```

**Pattern 3: Loop/Iteration**:

```python
# Refine output through multiple iterations
for iteration in range(3):
    output = refine_chain.invoke({"draft": output})
    print(f"Iteration {iteration + 1}: {output[:100]}...")
```

## =¡ Key Takeaways

- **Chaining breaks down complex tasks**: Easier to solve step-by-step
- **Each step builds on previous**: Pass outputs as inputs
- **Validation between steps**: Catch errors early
- **Output formatting matters**: Structure output for next step
- **Parallel chains are efficient**: Process independent steps together
- **Error handling is crucial**: Have fallbacks for failures
- **Documentation helps**: Log each step for debugging

## = Resources

- [LangChain Expression Language](https://python.langchain.com/docs/expression_language/)
- [Building Chains - LangChain](https://python.langchain.com/docs/modules/chains/)
- [Routing and Branching](https://python.langchain.com/docs/expression_language/how_to/routing)
- [Parallel Processing](https://python.langchain.com/docs/expression_language/how_to/parallel)
- [Example Chains - LangChain Hub](https://smith.langchain.com/hub)

##  Completion Checklist

- [ ] Understand why prompt chaining is useful
- [ ] Created a multi-step blog generation chain
- [ ] Built a code review chain
- [ ] Implemented a translation chain with verification
- [ ] Created a customer support chain
- [ ] Learned about conditional and parallel chaining
- [ ] Understand error handling in chains
- [ ] Can identify when to use chaining in real projects

## = Up Next

This marks the end of Week 3! You've covered vision models, embeddings, RAG, and LangChain fundamentals. Next week will dive into advanced topics like function calling, fine-tuning, and production deployment.

---

**Progress**: Day 20 of 30

**Week 3 Complete!** <‰

You've mastered:
- Vision models and multimodal AI
- CLIP for image embeddings
- Vector databases and semantic search
- RAG systems
- LangChain framework
- Prompt chaining for complex workflows
