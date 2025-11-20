# Day 4: Tokens & Context Windows <«

## =Ì Objective
Understand what tokens are, how they affect AI interactions, and why context windows matter for your prompts.

## <¯ Today's Exercise (10-15 minutes)

### Part 1: Understanding Tokens (5 min)

**What is a Token?**
- Tokens are pieces of words that AI models process
- ~4 characters = 1 token (rough estimate)
- Both input (prompt) and output (response) use tokens

**Examples**:
```
"Hello world" H 2 tokens
"ChatGPT is amazing" H 4 tokens
"Generative AI" H 3 tokens
"I'm" H 2 tokens (I + 'm)
```

**Try It Yourself**:
1. Visit [OpenAI Tokenizer](https://platform.openai.com/tokenizer)
2. Paste these sentences and see token counts:
   - "The quick brown fox jumps"
   - "The quick brown fox jumps over the lazy dog"
   - "AI is transforming technology"
   - Your own 20-word sentence

**Observe**:
- How do spaces affect tokens?
- What happens with punctuation?
- Do longer words = more tokens?

### Part 2: Context Windows Explained (5 min)

**What is a Context Window?**
- Maximum tokens a model can "remember" at once
- Includes: system prompt + conversation history + new input + output

**Model Comparison**:
```
GPT-3.5: 4K tokens (~3,000 words)
GPT-4: 8K-32K tokens (~6,000-24,000 words)
GPT-4 Turbo: 128K tokens (~96,000 words)
Claude 3: 200K tokens (~150,000 words)
Claude Opus: 200K tokens (~150,000 words)
Gemini 1.5 Pro: 1M tokens (~750,000 words)
```

**Practical Test**:
1. Open ChatGPT or Claude
2. Have a long conversation (10+ exchanges)
3. Ask: "What did we discuss at the very beginning?"
4. Notice: Models can "forget" early context when you exceed limits

### Part 3: Token Optimization (5 min)

**Exercise**: Optimize this prompt to use fewer tokens while keeping the meaning:

```
Original (verbose):
"I would really appreciate it if you could please help me understand
the various different ways in which I can optimize my prompts so that
they use fewer tokens while still maintaining the same level of
meaning and getting the same quality of response from the AI model."

Your optimized version:
[Try to reduce by 30% while keeping meaning]

Example answer:
"Explain how to optimize prompts to use fewer tokens while maintaining
quality and meaning."
```

**Token Saving Tips**:
- Remove filler words: "really", "various", "please"
- Use concise phrasing
- Avoid repetition
- But don't sacrifice clarity!

## =¡ Key Takeaways

- **Tokens = cost**: More tokens = higher API costs
- **Context limits exist**: Models can't remember infinite conversation
- **Efficiency matters**: Concise prompts save tokens and money
- **Newer models = larger context**: Technology improves rapidly
- **Token counting is imperfect**: Use it as a guideline, not exact science

## =' Real-World Implications

**Why This Matters**:
1. **API Costs**: OpenAI charges per token
2. **Speed**: Fewer tokens = faster responses
3. **Memory**: Long conversations may lose early context
4. **Task Planning**: Know if your document fits in context

**Cost Example**:
```
GPT-4 Pricing (example):
- Input: $0.03 per 1K tokens
- Output: $0.06 per 1K tokens

1,000-word document H 1,333 tokens
Analysis response H 500 tokens
Total cost: ~$0.07 per analysis
```

## = Resources

- [OpenAI Tokenizer Tool](https://platform.openai.com/tokenizer)
- [Understanding Tokens (OpenAI)](https://help.openai.com/en/articles/4936856-what-are-tokens)
- [Token Counting Libraries (tiktoken)](https://github.com/openai/tiktoken)

##  Completion Checklist

- [ ] Tested the tokenizer with various text samples
- [ ] Understand roughly how many tokens are in typical sentences
- [ ] Know the context window of your preferred AI model
- [ ] Practiced optimizing a prompt to use fewer tokens
- [ ] Understand why token efficiency matters for cost and performance

## = Up Next

Tomorrow: [Day 5 - OpenAI API - Your First Call](./day05.md) - Time to write some code! Make your first API call to GPT!

---

**Progress**: Day 4 of 30 
