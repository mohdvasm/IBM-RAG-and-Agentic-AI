✅ Key Concepts
📘 Definitions
🛠️ Examples
📌 Key Takeaways
🔑 Terms

---

# 🧠 Mastering LLM Agents with LangChain & LangGraph

**From Tools to Reasoning: A Complete Guide to Building Smarter AI Agents**

---

## 🧩 1. What Is the Difference Between an LLM and an Agent?

### 📘 Definition

* **LLM (Large Language Model)**: A neural network trained to predict and generate human-like text based on prompts.
* **Agent**: An LLM augmented with decision-making capabilities and access to external tools. It can **reason**, **act**, and **interact** with the real world.

### 📌 Key Concept

> A language model answers; an agent decides and acts.

### 🛠️ Example

* LLM: "What is 3 + 2?" → responds: "5" (if seen enough examples)
* Agent: Parses the question → calls an external tool to compute 3 + 2 → returns 5 reliably, even for complex or unseen queries.

---

## 🧰 2. What Are Tools?

### 📘 Definition

A **tool** is a function that performs a specific task (e.g., math, API calls, database queries) and is callable by an LLM agent.

### 🔑 Terms

* **Tool Class / Decorator**: In LangChain, Python functions are wrapped using `Tool` class or `@tool` decorator to make them usable by agents.
* **Structured Tool**: A tool with **typed inputs/outputs** (e.g., list of floats, booleans) to enable more accurate usage by LLMs.

### 🛠️ Example: Math Tool

```python
@tool
def add_numbers(numbers: List[float], absolute: bool = False) -> float:
    """Add a list of numbers. Optionally sum absolute values."""
    if absolute:
        return sum(abs(n) for n in numbers)
    return sum(numbers)
```

### 📌 Key Takeaways

* Tools transform LLMs into agents by enabling interaction with the external world.
* **Docstrings** are crucial: they describe input/output schemas and usage instructions for the LLM.

---

## 🔧 3. Structured Tools: Better Tool Interfaces

### 📘 Definition

**Structured tools** provide strict schemas that describe:

* **Name**: Derived from function name
* **Description**: Purpose of the tool
* **Args**: Input fields and types (must be JSON serializable)

### 🛠️ Example

```python
def add_with_options(numbers: List[float], absolute: bool = False) -> float:
    """Adds numbers. Set 'absolute' to True to sum absolute values."""
```

### 🔑 Terms

* `List`, `Dict`, `Union` from Python’s typing module define flexible and accurate input/output types.
* LLMs often work better with **typed JSON** rather than free-text.

### 📌 Key Takeaways

* Structured tools improve compatibility with **function-calling models** (e.g., OpenAI, IBM Granite).
* Enable **multi-parameter inputs** and **richer outputs**.

---

## 🧠 4. Agents in LangChain: From Reasoning to Acting

### 📘 Definition

An **agent** is a combination of:

* One or more tools
* An LLM
* A strategy for **reasoning + tool calling + response generation**

### 🔑 Terms

* **initialize\_agent**: Quickly bootstraps agents from tools and LLMs
* **Zero-Shot ReAct Agent**: A reasoning loop that works without training examples

### 🧩 Agent Workflow

1. Receive a prompt.
2. Think: Decide which tool(s) to use.
3. Act: Call the tool.
4. Observe: Analyze the output.
5. Reflect: Possibly loop again.
6. Respond: Return the final answer.

### 🛠️ Example

```python
agent = initialize_agent(
    tools=[add_tool],
    llm=llm,
    agent="zero-shot-react-description",
    verbose=True
)
agent.run("What is the sum of 3, 4, and 5?")
```

---

## 🛠️ 5. LangGraph: Custom Agent Workflows

### 📘 Definition

**LangGraph** is a newer, more flexible framework for creating custom agents. Offers full control over:

* Agent logic
* Reasoning steps
* Tool orchestration

### 🔑 Terms

* `create_react_agent`: LangGraph’s method to build a ReAct-style agent
* `invoke()`: Call agents with structured input, e.g., a dictionary of messages

### 🛠️ Example

```python
agent = create_react_agent(llm, tools=[add, multiply, search_wiki])
response = agent.invoke({"messages": [{"role": "user", "content": "What is 7x3?"}]})
```

---

## 🔗 6. Building a Math Toolkit Agent

### 🧩 Toolkit Tools

* `add_numbers`
* `subtract_numbers`
* `multiply_numbers`
* `divide_numbers`

Combine all in one agent:

```python
tools = [add_tool, subtract_tool, multiply_tool, divide_tool]
agent = create_react_agent(llm, tools)
```

### 🧠 Agent Use Case

> Input: “What is 20 / 4 + 3?”
> Agent reasons → selects tools → returns answer.

---

## 🌍 7. Hybrid Tools: Combine Math + Information Retrieval

### 📘 Use Case

Build an agent that:

* Fetches population of Canada (Wikipedia tool)
* Multiplies by 0.75

### 🛠️ Steps

1. Tool 1: Wikipedia Search
2. Tool 2: Multiply Numbers
3. Agent uses both tools in sequence

### 📌 Key Takeaway

> Agents can orchestrate **multi-tool**, **multi-step** workflows, blending logic, math, and external knowledge.

---

## 🧠 Final Summary: What You’ve Learned

| Feature         | LangChain                    | LangGraph                             |
| --------------- | ---------------------------- | ------------------------------------- |
| Tool Definition | `@tool` or `Tool()`          | Same                                  |
| Agent Init      | `initialize_agent()`         | `create_react_agent()`                |
| Input Format    | Mostly strings or basic JSON | Full structured input                 |
| Best For        | Quick prototyping            | Custom workflows, fine-tuned behavior |

---

## ✅ Key Takeaways Recap

| # | Concept               | Takeaway                                                 |
| - | --------------------- | -------------------------------------------------------- |
| 1 | **Tools**             | Functions LLMs can call to interact with real-world data |
| 2 | **Structured Tools**  | Enable complex inputs/outputs with JSON schemas          |
| 3 | **Agents**            | Combine tools + LLM + reasoning loop                     |
| 4 | **ReAct**             | Reason + Act → Observe → Repeat                          |
| 5 | **LangGraph**         | More control over multi-step agent workflows             |
| 6 | **invoke()**          | Better debugging and testing tool interactions           |
| 7 | **Multi-Tool Agents** | Orchestrate logic + retrieval + computation              |

