# Day 27: LangChain Agents

## =Ì Objective
Learn how to build autonomous AI agents using LangChain that can use tools, make decisions, and take actions to accomplish complex tasks.

## <¯ Today's Exercise (10-15 minutes)

### Prerequisites
- Python 3.8+
- `pip install langchain langchain-openai python-dotenv`
- OpenAI API key

### Part 1: Understanding AI Agents (2 min)

**What is an AI Agent?**

An AI agent is an autonomous system that:
- Takes user input/goals
- Breaks down tasks into steps
- Uses available tools to gather information
- Makes decisions based on results
- Continues until goal is achieved
- Reports back with final answer

**Agent vs Chain**:

```
CHAIN (Sequential):
Input ’ LLM ’ Output
(Fixed sequence, no thinking)

AGENT (Intelligent):
Input ’ Think ’ Choose Tool ’ Execute ’ Observe ’ Think ’ Choose ’ ... ’ Output
(Dynamic, can adapt based on results)
```

**Key Components**:

1. **Agent**: Makes decisions about which actions to take
2. **Tools**: Functions the agent can call (search, calculator, API, etc.)
3. **Memory**: Remembers past interactions
4. **Planner**: Decides steps to accomplish goal
5. **Executor**: Runs chosen actions

### Part 2: Building Simple Agent (5 min)

**Create `agent_demo.py`**:

```python
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain.agents import (
    create_react_agent,
    AgentExecutor,
    Tool
)
from langchain import hub
from langchain.tools import tool

load_dotenv()

print("="*60)
print("LANGCHAIN AGENTS EXAMPLES")
print("="*60)

# ============================================================
# SETUP: Define Tools
# ============================================================

print("\nDefining Tools...")
print("-" * 40)

@tool
def multiply(a: int, b: int) -> int:
    """Multiply two numbers together"""
    return a * b

@tool
def divide(a: int, b: int) -> float:
    """Divide two numbers"""
    if b == 0:
        return "Cannot divide by zero"
    return a / b

@tool
def add(a: int, b: int) -> int:
    """Add two numbers together"""
    return a + b

@tool
def subtract(a: int, b: int) -> int:
    """Subtract two numbers"""
    return a - b

@tool
def get_today_date() -> str:
    """Get today's date"""
    from datetime import datetime
    return datetime.now().strftime("%Y-%m-%d")

@tool
def web_search(query: str) -> str:
    """Search the web for information (mock)"""
    # In a real app, this would call a real search API
    search_results = {
        "what is AI": "AI is Artificial Intelligence, machine learning...",
        "python tutorials": "Python tutorials available on datacamp, coursera...",
        "machine learning": "ML is a subset of AI that focuses on learning from data..."
    }
    return search_results.get(query.lower(), "No results found")

# Create tools list
tools = [multiply, divide, add, subtract, get_today_date, web_search]

print(f"Created {len(tools)} tools:")
for tool_obj in tools:
    print(f"  - {tool_obj.name}: {tool_obj.description}")

# ============================================================
# EXAMPLE 1: SIMPLE MATH AGENT
# ============================================================

print("\n" + "="*60)
print("EXAMPLE 1: Math Problem Solver")
print("-" * 40)

# Initialize LLM
llm = ChatOpenAI(
    model="gpt-3.5-turbo",
    temperature=0,
    api_key=os.getenv("OPENAI_API_KEY")
)

# Get prompt template
prompt = hub.pull("hwchase17/react")

# Create agent
agent = create_react_agent(llm, tools, prompt)

# Create executor
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=True,
    max_iterations=5
)

# Run agent
print("\nMath Problem: What is (50 * 2) + 100?\n")
response = agent_executor.invoke({
    "input": "What is (50 * 2) + 100?"
})

print(f"\nFinal Answer: {response['output']}\n")

# ============================================================
# EXAMPLE 2: INFORMATION GATHERING AGENT
# ============================================================

print("="*60)
print("EXAMPLE 2: Information Gathering")
print("-" * 40)

print("\nQuery: What are some good Python resources?\n")
response = agent_executor.invoke({
    "input": "What are some good Python learning resources? Search for them and tell me."
})

print(f"\nAnswer: {response['output']}\n")

# ============================================================
# EXAMPLE 3: COMPLEX REASONING
# ============================================================

print("="*60)
print("EXAMPLE 3: Complex Multi-step Problem")
print("-" * 40)

complex_prompt = """
I have a project budget of $10,000.
I need to spend 40% on development, 30% on marketing, and 20% on operations.
How much is left for contingency?
Calculate this for me step by step.
"""

print(f"\nQuery: {complex_prompt}\n")
response = agent_executor.invoke({
    "input": complex_prompt
})

print(f"\nAnswer: {response['output']}\n")

# ============================================================
# AGENT TYPES EXPLANATION
# ============================================================

print("="*60)
print("DIFFERENT AGENT TYPES")
print("="*60)

agent_types = """
1. ReAct Agent (Reasoning + Acting)
   - Used above, most flexible
   - Good for: Complex reasoning, multiple tools
   - Format: Thought ’ Action ’ Observation ’ Thought

2. Tool Calling Agent
   - Uses function calling in newer models
   - Good for: Structured tool use
   - Fast and reliable

3. OpenAI Function Calling Agent
   - Leverages GPT's function calling
   - Good for: When you have many tools
   - Most modern approach

4. Conversational Agent
   - Remembers chat history
   - Good for: Chatbots, multi-turn conversations
   - Can reference previous messages

5. Plan and Execute Agent
   - Separates planning from execution
   - Good for: Large complex tasks
   - Better for long chains of reasoning
"""

print(agent_types)

# ============================================================
# AGENT WITH MEMORY
# ============================================================

print("="*60)
print("EXAMPLE 4: Agent with Memory")
print("-" * 40)

from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory(memory_key="chat_history")

memory_example = """
from langchain.agents import create_react_agent, AgentExecutor
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory(memory_key="chat_history")

agent_with_memory = AgentExecutor(
    agent=agent,
    tools=tools,
    memory=memory,
    verbose=True
)

# Multi-turn conversation
agent_with_memory.invoke({"input": "What is 5 + 3?"})
agent_with_memory.invoke({"input": "What was the previous result times 2?"})
"""

print("Agent with Memory Example:")
print(memory_example)

# ============================================================
# BEST PRACTICES
# ============================================================

print("\n" + "="*60)
print("AGENT BEST PRACTICES")
print("="*60)

best_practices = """
1. TOOL DESIGN
   - Clear, descriptive tool names and descriptions
   - One tool per function
   - Return structured data
   - Handle errors gracefully

2. PROMPT ENGINEERING
   - Give agent clear instructions
   - Specify format of responses
   - Provide examples of reasoning
   - Set constraints (max steps, budget)

3. SAFETY & CONSTRAINTS
   - Set max iterations to prevent infinite loops
   - Validate tool outputs
   - Log all agent decisions
   - Monitor tool usage and costs

4. TESTING & MONITORING
   - Test with various inputs
   - Monitor agent decision-making
   - Track success rates
   - Iterate on tool and prompt

5. OPTIMIZATION
   - Use faster models for simple tasks
   - Cache results when possible
   - Parallel tool execution
   - Optimize tool selection

6. SCALABILITY
   - Use appropriate agent type for task
   - Distribute tools across services
   - Use async execution
   - Implement proper error handling
"""

print(best_practices)

# ============================================================
# REAL-WORLD USE CASES
# ============================================================

print("\n" + "="*60)
print("REAL-WORLD AGENT USE CASES")
print("="*60)

use_cases = """
1. CUSTOMER SUPPORT AGENT
   Tools: FAQ search, order lookup, email send, escalation
   Task: Resolve customer issues autonomously

2. RESEARCH ASSISTANT
   Tools: Web search, document parsing, summarization
   Task: Gather and summarize information on topic

3. CODE DEBUGGER
   Tools: Code analysis, error lookup, test running
   Task: Find and suggest fixes for bugs

4. SALES AGENT
   Tools: CRM access, product catalog, email, calendar
   Task: Qualify leads and schedule meetings

5. DATA ANALYST
   Tools: Database query, visualization, statistical analysis
   Task: Answer business questions from data

6. TRAVEL PLANNER
   Tools: Flight search, hotel booking, weather API, maps
   Task: Plan complete trip based on preferences
"""

print(use_cases)
```

**Run it**:
```bash
python agent_demo.py
```

### Part 3: Creating Custom Agents (3 min)

**Key Agent Patterns**:

```python
# Pattern 1: Simple tool-calling agent
agent = create_react_agent(llm, tools, prompt_template)
executor = AgentExecutor(agent=agent, tools=tools)

# Pattern 2: Conversational agent with memory
from langchain.memory import ConversationBufferMemory
memory = ConversationBufferMemory()
executor = AgentExecutor(..., memory=memory)

# Pattern 3: Structured output agent
from langchain.agents.structured_chat.output_parser import StructuredChatOutputParser
# Returns JSON with specific schema

# Pattern 4: Multi-agent system
from langchain.agents import AgentType
# Multiple agents collaborate to solve problems
```

## =¡ Key Takeaways

- **Agents are more flexible than chains** - can adapt based on intermediate results
- **Tool design is critical** - clear descriptions help agents use tools correctly
- **ReAct pattern works well** - explicit reasoning step improves decision-making
- **Memory enables conversations** - agents can reference previous interactions
- **Safety constraints matter** - prevent infinite loops and wasted tokens
- **Iterative refinement needed** - test with various inputs and refine
- **Cost monitoring essential** - agents can make many LLM calls
- **Real-world applications are powerful** - support, research, debugging, planning

## = Resources

- [LangChain Agents Documentation](https://python.langchain.com/docs/modules/agents/)
- [ReAct Paper](https://arxiv.org/abs/2210.03629)
- [Agent Types Guide](https://python.langchain.com/docs/modules/agents/agent_types/)
- [Building Custom Tools](https://python.langchain.com/docs/modules/agents/tools/)
- [Agent Executor](https://python.langchain.com/docs/modules/agents/concepts/#agent-executor)

##  Completion Checklist

- [ ] Understand the difference between chains and agents
- [ ] Know the components of an agent
- [ ] Created custom tools using @tool decorator
- [ ] Built a ReAct agent for problem solving
- [ ] Used agent executor to run agents
- [ ] Understood different agent types
- [ ] Added memory to agents for conversations
- [ ] Know how to handle agent errors and constraints
- [ ] Understand real-world agent applications
- [ ] Can design agents for specific problems

## =­ Challenge

Build an autonomous agent for:
1. **Customer Support Agent**: Answers FAQs, looks up orders, escalates issues
2. **Research Agent**: Searches for information and summarizes findings
3. **Code Helper Agent**: Explains code, suggests optimizations, finds bugs

## = Up Next

Tomorrow: [Day 28 - Prompt Injection & AI Safety](./day28.md) - Learn to secure your AI systems against attacks!

---

**Progress**: Day 27 of 30

**Almost there!** You're building sophisticated AI systems. Just 3 days left!
