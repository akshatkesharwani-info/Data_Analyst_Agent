# Data Analyst Agent

An AI agent that performs an end-to-end data analysis workflow on its own -- decides which
steps to take, in what order, rather than following a fixed script.

## Problem
A small business doesn't have a dedicated analyst. This points a LangGraph agent at a raw sales
CSV with a single natural-language goal and lets it plan and execute the analysis itself: load
data, compute KPIs, generate a chart, write a report.

## What It Does
- Exposes four tools to the agent: `load_dataset`, `compute_kpis`, `generate_sales_chart`,
  `write_report`
- The agent (not a hardcoded pipeline) decides the order to call them in based on the goal
- Uses `ChatOpenAI` pointed at Groq's OpenAI-compatible endpoint, since `create_react_agent`
  needs a model that supports real tool-calling
- Ends with an LLM-written narrative report referencing the actual computed numbers, not a
  templated summary

## Real Results (real dataset, 9,994 Superstore order-line rows)
Given the single instruction "load the data, compute KPIs, generate a chart, and write a
report," the agent correctly planned and executed all four steps and returned:
- **Total Sales: $2,297,200.86**
- **Total Profit: $286,397.02** (~12.5% margin)
- **Average Order Value: $229.86**
- **Top region: West** | **Top category: Technology**
- A saved bar chart (`agent_sales_by_region.png`) and a written report with specific,
  numbers-backed recommendations (deep-dive the West region, invest further in Technology,
  review discount structure to lift margin above 12.5%)

## Tech Stack
Python, LangGraph, LangChain, Cerebras / Groq (free tier), Pandas, Matplotlib

## How to Run
Open in Google Colab, run all cells, enter a free Cerebras and Groq API key when prompted.
Dataset auto-downloads via `kagglehub` with a synthetic fallback if it ever fails.
