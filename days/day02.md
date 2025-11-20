# Day 2: ChatGPT - Prompt Engineering Basics =¨

## =Ã Objective
Master fundamental prompt engineering techniques using ChatGPT to get better, more consistent results from AI.

## <Ø Today's Exercise (10-15 minutes)

### The PREP Framework

Learn the **PREP** framework for effective prompting:
- **P**rompt: Clear instruction
- **R**ole: Who should the AI be?
- **E**xpectation: What format/length/style?
- **P**arameters: Any constraints or requirements?

### Hands-On: Practice Each Technique

**1. Role Prompting (3 min)**

Try both and compare:

```
Bad: "Explain blockchain"

Good: "You are a tech educator teaching beginners. Explain blockchain
in simple terms using an everyday analogy. Keep it under 100 words."
```

**2. Few-Shot Prompting (4 min)**

Give examples to guide the AI:

```
Convert these movie titles to emojis:

Movie: Titanic
Emoji: =¢D=î

Movie: The Lion King
Emoji: >Å=Q<

Movie: Star Wars
Emoji: Pî=Ä

Movie: Inception
Emoji: [Let AI complete]
```

**3. Chain of Thought (3 min)**

Ask the AI to think step-by-step:

```
"Let's solve this step by step:

If a train leaves Station A at 60 mph at 2:00 PM and another train
leaves Station B (120 miles away) at 40 mph at 2:30 PM traveling
toward each other, when will they meet?

Please show your reasoning at each step."
```

**4. Output Formatting (3 min)**

Specify exact format:

```
"Analyze the pros and cons of remote work. Format as:

PROS:
- [point 1]
- [point 2]
- [point 3]

CONS:
- [point 1]
- [point 2]
- [point 3]

VERDICT: [one sentence]"
```

## =° Key Takeaways

- **Specific roles** improve relevance (teacher, developer, analyst, etc.)
- **Examples teach** the AI your desired format (few-shot learning)
- **Step-by-step reasoning** improves accuracy for complex tasks
- **Clear format requirements** ensure consistent, usable output
- **Iterate and refine** - prompt engineering is experimental!

## =' Pro Tips

1. **Be specific about length**: "in 3 sentences" vs "briefly"
2. **Define the audience**: "for a 10-year-old" vs "for experts"
3. **Set the tone**: "professional", "casual", "humorous"
4. **Use delimiters**: Triple quotes, XML tags, or markdown for structure

## = Resources

- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [Learn Prompting - Free Course](https://learnprompting.org/)
- [ChatGPT Prompt Examples](https://prompts.chat/)

##  Completion Checklist

- [ ] Tried role prompting with 2+ different roles
- [ ] Created a few-shot prompt with examples
- [ ] Used chain-of-thought for a multi-step problem
- [ ] Formatted output in a specific structure
- [ ] Saved your best prompts for future use

## = Up Next

Tomorrow: [Day 3 - Claude & Understanding AI Models](./day03.md) - Explore different AI models and when to use each!

---

**Progress**: Day 2 of 30 
