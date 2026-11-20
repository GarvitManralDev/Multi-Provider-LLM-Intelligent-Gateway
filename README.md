# Multi-Provider LLM Intelligent Gateway

> 📚 **This is a learning project.** It isn't a production-ready app or a library. It's a notebook where I explored how LLM gateways work.

## What I Was Trying to Learn

Each LLM provider (OpenAI, Anthropic, Gemini, Groq) has its own SDK. Wondering how real apps avoid getting locked into one of them, and what happens when that provider goes down.

The answer is an **LLM gateway**: a layer between the app and the providers. To understand the idea, I used [LiteLLM](https://github.com/BerriAI/litellm) to try out the common gateway features one by one.

```text
  App  ──▶  LiteLLM (gateway)  ──▶  OpenAI / Anthropic / Gemini / Groq
```

## What's in the Notebook

Everything is in `llm_gateway.ipynb`, written as small experiments:

1. **One API for every provider:** the same `completion()` call with a different `model` string
2. **Fallbacks:** making a model fail on purpose and watching the next one take over
3. **Cost tracking:** checking tokens and the cost of a call
4. **Caching:** asking the same question twice; the second call is served from the cache
5. **Routing:** aliases like `fast-cheap` and `smart-coding` that map to real models
6. **Load balancing:** spreading requests across deployments (`simple-shuffle`, `least-busy`)
7. **LangChain + LiteLLM:** using `ChatLiteLLM` in a chain, with fallbacks
8. **A mini task-aware chatbot:** classifies a query, picks a model, and logs latency and cost
9. **Simple guardrails:** regex and keyword checks for PII, prompt injection and blocked topics

At the end there's a quick comparison table of other gateways (Portkey, Helicone, OpenRouter, etc.).

## Running It Yourself

```bash
conda create -n llm-gateway python=3.11 -y
conda activate llm-gateway
pip install -r requirements.txt

cp .env.example .env   # then add your API keys
jupyter notebook llm_gateway.ipynb
```

Most cells only need **OpenAI** and **Groq** keys. The Anthropic and Gemini cells will just show an error if you leave those keys out, which also shows the fallbacks working.

## Things to Keep in Mind

- These are **learning experiments**, so the code is kept simple on purpose.
- The guardrails are basic regex and keyword checks. They're useful for understanding the idea, not real protection.
- Some model names may be outdated by the time you read this. Swap in current ones if a cell fails.
- Running the notebook makes real API calls, so it costs a little money.

## Key Takeaways

- Code against **one interface**, not a specific vendor's SDK.
- **Fallbacks** are the easiest way to survive provider outages.
- **Routing** cheap tasks to cheap or fast models saves real money.
- Caching, cost tracking and guardrails all fit naturally in the gateway layer.

---

*Made for learning. Feedback and suggestions are welcome!*
