# What does it do?

LangChain allows users to orchestrate LLMs. It solves the issue of switching between LLMs whenever the need arises and helps to keep a buffer/memory of the responeses. In a way, it orchestrates all different components that need to work together:

- The LLM (GPT, Claude, etc.);
- Tools the LLM can use (vector database search, weather API, calculators, etc.);
- Memory (to remember the conversation history);
- Prompts (how you format question to the LLM);
- Documentation loaders (to read PDFs, websites, etc);
- And connecting all these pieces together in chains;

```
Step 1: User input/question -> vector database search tool -> get relevant docs
    ↓
Step 2: Format a prompt with the question + retrieved docs
    ↓
Step 3: Send to LLM
    ↓
Step 4: Return answer
```