# What is RAG?

RAG is an acronym for Retrieval-Augmented Generation. In short, you provide extra context in the form of, for example, documents to the LLM and it has context to answer your questions.

# How RAG works?

1. **Retrieve** relevant information from a knowledge base (your docs, databases, PDFs, etc.)
2. **Augment** the LLM prompt with that retrieved information
3. **Generate** a response based on the provided context

Instead of asking LLM directly:

```
User: "What's our company's refund policy?"
LLM: "I don't have access to your company's specific policies..."
```

The user provides context for the LLM to get a more precise answer/correct:

```
1. Search your company docs for "refund policy"
2. Find relevant sections
3. Send to LLM: "Given this policy: [retrieved text], answer: What's our refund policy?"
LLM: "According to your policy, refunds are available within 30 days..."
```

## Architecture workflow

```
User Question
    ↓
[Vector Database Search] ← Your documents stored as embeddings
    ↓
Retrieve top relevant chunks
    ↓
[LLM] ← Question + Retrieved context
    ↓
Generated Answer (grounded in your data)
```

## The Problem It Solves:

The retrieval-augmented generation (RAG) approach helps solve several challenges in natural language processing (NLP) and AI applications:

1. **Factual Inaccuracies and Hallucinations**: Traditional generative models can produce plausible but incorrect information. RAG reduces this risk by retrieving verified, external data to ground responses in factual knowledge.
2. **Outdated Information**: Static models rely on training data that may become obsolete. RAG dynamically retrieves up-to-date information, ensuring relevance and accuracy in real-time****.****
3. **Contextual Relevance**: Generative models often struggle with maintaining context in complex or multi-turn conversations. RAG retrieves relevant documents to enrich the context, improving coherence and relevance.
4. **Domain-Specific Knowledge**: Generic models may lack expertise in specialized fields. RAG integrates domain-specific external knowledge for tailored and precise responses.
5. **Cost and Efficiency**: Fine-tuning large models for specific tasks is expensive. RAG eliminates the need for retraining by dynamically retrieving relevant data, reducing costs and computational load.
6. **Scalability Across Domains**: RAG is adaptable to diverse industries, from healthcare to finance, without extensive retraining, making it highly scalable

___
### Sources

1. https://www.geeksforgeeks.org/nlp/what-is-retrieval-augmented-generation-rag/
2. https://aws.amazon.com/what-is/retrieval-augmented-generation/