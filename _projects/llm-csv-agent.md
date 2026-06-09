---
title: "AI: LLM CSV Agent"
date: 2024-11-10
status: "Prototype"
field: "AI"
tools:
  - Python
  - LangChain
  - LangChain Experimental
  - Ollama
  - DeepSeek Coder
  - Pandas
  - CSV
github: "https://github.com/mohamed1249/LLM-CSV-Agent"
demo:
kaggle:
excerpt: "A local LLM-powered CSV agent that uses LangChain and Ollama to answer analytical questions over tabular data, turning natural-language prompts into pandas-style reasoning."
---

## The Short Version

LLM CSV Agent is a local AI prototype for asking questions about CSV data in natural language. Instead of manually writing pandas code for every small analysis task, the project uses a LangChain CSV agent connected to a local Ollama model so the user can ask questions like: "Sort the seasons by the number of episodes they have."

## Problem

CSV files are everywhere, but exploring them still often requires knowing the right pandas commands. This project experiments with a more conversational workflow:

- load a CSV file,
- connect it to an LLM agent,
- ask an analytical question in plain English,
- let the agent reason over the table,
- return the result.

The goal is not to replace careful analysis, but to make first-pass exploration faster and more accessible.

## Approach

**CSV agent** - The core script uses `create_csv_agent` from `langchain_experimental.agents.agent_toolkits` to give an LLM access to a CSV file through a pandas-aware tool.

**Local model** - The agent is powered through `ChatOllama`, using the `deepseek-coder-v2` model locally instead of relying on a hosted API.

**Structured output attempt** - The project experiments with `JsonOutputParser` and parsing-error handling to make the agent output easier to consume programmatically.

**Dataset** - The local prototype runs against `episode_info.csv`, then asks the agent to sort seasons by the number of episodes.

**Exploration notes** - The folder also includes pandas reference notes and a separate RAG experiment, showing the project as part of a larger learning path around LangChain, data analysis, and local LLM tooling.

## Result

The working prototype demonstrates a natural-language interface over CSV data. It connects a local LLM to a structured dataset, runs an analytical request, and prints the result with verbose agent tracing enabled for debugging.

It is a good early project because it touches several important ideas at once: tool-using agents, local LLMs, tabular reasoning, parser reliability, and the safety tradeoffs of allowing an agent to execute dataframe operations.

## What I Learned

This project shows both the promise and the risk of LLM data agents. They can make data exploration feel much more natural, but they need careful guardrails, especially when `allow_dangerous_code=True` is enabled. A production version would need safer execution, clearer schemas, better output validation, file upload support, and a more polished interface.

It also helped connect pandas knowledge with agentic AI: the better you understand dataframe operations, the better you can judge whether the agent's reasoning and output are trustworthy.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
