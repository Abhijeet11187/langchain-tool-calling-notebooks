# 🔧 LangChain Tool Calling — From Basics to Real-World Integration

A structured, hands-on notebook series covering **tool calling with LangChain and OpenAI**, progressing from foundational concepts to a fully working real-world currency conversion agent. Each notebook builds on the previous one, making this series ideal for developers learning how LLMs interact with external tools and APIs.

---

## 📁 Repository Structure

```
├── creating_basic_custom_tools_langchain.ipynb   # Notebook 1 — Creating custom tools (3 methods)
├── tool_calling_langchain.ipynb                  # Notebook 2 — Tool binding & execution flow
├── currency_conversion_tool_calling.ipynb        # Notebook 3 — Real-world multi-tool agent
└── README.md
```

---

## 📓 Notebook Descriptions

### 1. `creating_basic_custom_tools_langchain.ipynb` — Creating Basic Custom Tools

This foundational notebook introduces **three different methods** for creating custom tools in LangChain, all demonstrated using a simple `multiply` function as a consistent example.

**What you'll learn:**

- **Method 1 — `@tool` Decorator**: The simplest and most Pythonic approach. Add type hints and a docstring to any function, then decorate it with `@tool`. LangChain automatically generates the name, description, and JSON schema that gets sent to the LLM.

- **Method 2 — `StructuredTool` with Pydantic**: Use `StructuredTool.from_function()` paired with a `Pydantic` `BaseModel` to define a strict, validated input schema. Ideal when you need fine-grained control over input fields, descriptions, and validation rules.

- **Method 3 — `BaseTool` Class**: The most flexible, low-level approach. Subclass `BaseTool` directly to define a tool as a class, giving you full control over tool behavior, schema, and execution logic. All other tool types in LangChain are built on top of this.

**Key concepts covered:**
- Tool attributes: `name`, `description`, `args`, `args_schema`
- JSON schema generation (`model_json_schema`) and how it is sent to the LLM
- When to use each method based on use case complexity

---

### 2. `tool_calling_langchain.ipynb` — Tool Binding & Execution Flow

This notebook covers the complete **tool calling lifecycle** — from binding a tool to an LLM, to executing it, to feeding results back into the conversation.

**What you'll learn:**

- **Tool Binding**: How to attach tools to an LLM instance using `llm.bind_tools([...])`, creating an LLM that is aware of the available tools.

- **LLM Behavior with Tools**: Understanding the difference between a response with content (no tool needed) and a response with `tool_calls` (LLM delegates to a tool). The LLM does **not** execute the tool — it only suggests the tool and the arguments.

- **Tool Execution**: How to programmatically invoke the suggested tool using `tool.invoke(result.tool_calls[0])`.

- **Conversation History Management**: How to maintain a list of messages (user query → LLM response → tool result → final LLM response) so the model can generate a coherent final answer using the tool's output.

**Key concepts covered:**
- `HumanMessage` and message list construction
- `tool_calls` field in LLM responses
- Appending tool results to the conversation history
- Full round-trip: user → LLM → tool → LLM → final answer

---

### 3. `currency_conversion_tool_calling.ipynb` — Real-World Multi-Tool Agent

The most advanced notebook in the series. This builds a **real-world currency conversion agent** using two coordinated tools and a live exchange rate API, demonstrating `InjectedToolArg` and multi-turn tool orchestration.

**What you'll learn:**

- **Tool 1 — `get_conversion_factor`**: Calls the [ExchangeRate-API](https://www.exchangerate-api.com/) to fetch the live conversion rate between any two currencies.

- **Tool 2 — `convert`**: Takes a base currency amount and a conversion rate to compute the target currency value. Uses `InjectedToolArg` to inject the `conversion_rate` programmatically — the LLM is not expected to supply this value; the developer injects it from the first tool's result.

- **Multi-Tool Orchestration**: How to loop over `tool_calls` in an LLM response, identify which tool to run, pass results between tools, and build up the full conversation history across multiple turns.

- **`InjectedToolArg`**: A LangChain utility that marks a tool argument as developer-injected, preventing the LLM from hallucinating or guessing its value.

**Key concepts covered:**
- Live API integration inside LangChain tools
- `InjectedToolArg` and `Annotated` typing
- Chaining dependent tools with shared state (conversion rate)
- Multi-turn conversation management with tool messages
- Parsing JSON from tool results

---

## ⚙️ Prerequisites

- Python 3.8+
- An [OpenAI API key](https://platform.openai.com/api-keys)
- An [ExchangeRate-API key](https://www.exchangerate-api.com/) *(required only for Notebook 3)*

---

## 📦 Installation

Install the required dependencies:

```bash
pip install langchain langchain-openai langchain-core requests pydantic
```

Or install from within any notebook cell:

```python
!pip install langchain langchain-openai langchain-core requests
```

---

## 🚀 Getting Started

1. **Clone the repository**

   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```

2. **Open the notebooks in order**

   Start with `creating_basic_custom_tools_langchain.ipynb`, then proceed to `tool_calling_langchain.ipynb`, and finally `currency_conversion_tool_calling.ipynb`.

3. **Set your API keys when prompted**

   The notebooks use `getpass` to securely accept API keys at runtime — no `.env` file or hardcoding required.

---

## 🧠 Concepts at a Glance

| Concept | Notebook |
|---|---|
| `@tool` decorator | Notebook 1 |
| `StructuredTool` with Pydantic | Notebook 1 |
| `BaseTool` subclassing | Notebook 1 |
| `bind_tools` — tool binding | Notebook 2 |
| Tool execution & `tool_calls` | Notebook 2 |
| Conversation history management | Notebook 2 |
| Live API integration in tools | Notebook 3 |
| `InjectedToolArg` | Notebook 3 |
| Multi-tool orchestration | Notebook 3 |
| Multi-turn agent loop | Notebook 3 |

---

## 🛠️ Tech Stack

- [LangChain](https://www.langchain.com/) — LLM orchestration framework
- [LangChain OpenAI](https://python.langchain.com/docs/integrations/llms/openai/) — OpenAI model integration
- [OpenAI GPT-4o-mini](https://platform.openai.com/) — Underlying language model
- [ExchangeRate-API](https://www.exchangerate-api.com/) — Live currency exchange rates
- [Pydantic](https://docs.pydantic.dev/) — Input schema validation

---

## 📌 Notes

- The LLM **suggests** tool calls but does **not** execute them. Execution is always handled by LangChain or your own code.
- Always maintain conversation history as a list of messages when building multi-turn tool interactions.
- `InjectedToolArg` is particularly useful when a tool argument should be derived from another tool's output rather than generated by the LLM.

---


## 📄 License

This project is for educational and demonstration purposes. Feel free to fork and extend.
This project is for educational purposes. Feel free to use and adapt it for your own learning.



## ⭐ If you found this helpful

Give this repository a star ⭐ 
