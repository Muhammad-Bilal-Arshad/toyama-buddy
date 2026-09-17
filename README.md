# Toyama Buddy

A weather-aware AI travel guide for foreign visitors to Toyama, Japan — built with an LLM, RAG (retrieval-augmented generation), and an Agent with live tool use.

## What it does

Toyama Buddy answers tourism questions using a curated knowledge base of Toyama attractions, food, transport, and events, and always cites its sources. Before recommending any outdoor activity — like the Tateyama Kurobe Alpine Route or Toyama Castle Park — it automatically checks the live weather forecast and adapts its recommendation, suggesting an indoor alternative (like the Toyama Glass Art Museum) if conditions are bad. It replies in whatever language the visitor writes in, tested in both English and Japanese.

**Example:** ask "I want to see the Alpine Route tomorrow, what should I do?" and the agent checks the live forecast for the Murodo mountain area before answering, rather than guessing.

## Tech stack

- **LLM:** Google Gemini (`gemini-3.1-flash-lite`), via the `google-genai` Python SDK
- **RAG:** 14 curated Toyama tourism documents, embedded with `intfloat/multilingual-e5-small` (supports English + Japanese) and stored in ChromaDB
- **Agent:** Gemini's automatic function calling — the model decides on its own when to call each tool
  - `search_knowledge_base(query)` — retrieves relevant passages from the knowledge base
  - `get_weather(location)` — live 3-day forecast from the Open-Meteo API (no key required), for either Toyama City or the Alpine Route's Murodo area
- **UI:** Gradio, launched with a shareable public link for live demos
- **Environment:** Google Colab

## Evaluation

We compared plain Gemini (no RAG, no tools) against the full agent on 6 test questions. Full results are in [`evaluation.md`](./evaluation.md) and [`evaluation_chart.png`](./evaluation_chart.png).

**Headline result:** asked "What is today's weather in Toyama?", plain Gemini invented a date and fabricated weather conditions. Toyama Buddy pulled the real forecast from a live API call.

- Toyama Buddy used a tool on **6/6** questions
- Source citation rate: plain LLM **2/6** vs. Toyama Buddy **5/6**

## How to run

1. Open [`toyama_buddy.ipynb`](./toyama_buddy.ipynb) in [Google Colab](https://colab.research.google.com).
2. Get a free API key from [Google AI Studio](https://aistudio.google.com).
3. In Colab, click the 🔑 (Secrets) icon in the left sidebar → **Add new secret** → name it `GEMINI_API_KEY` → paste your key → enable **Notebook access**.
4. Run all cells (**Runtime → Run all**). The last cell launches a Gradio chat interface with a public `gradio.live` link.

## Repository contents
