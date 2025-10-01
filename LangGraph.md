## What is LangGraph? 

[[LangChain]] handles simple linear chains (step 1 -> step 2 -> step 3). But if one needs:

- Loops (retry it it fails);
- Conditional branching (if X happens, do A; otherwise do B);
- Multi-agent systems (multiple AI agents collaborating);
- Complex state management;

LangGraph allows us to build these as a **graph** (nodes and edges), where each node is a step and edges determine the flow.

Example:

1. Writes code;
2. Tests it;
3. If tests fail -> go back to step 1 and fix;
4. If tests pass -> deploy;

The code below exemplifies how to declare the main class to build graphs.

```Python
from langgraph.graph import StateGraph, END

# Define your state structure
class GraphState(TypedDict):
    messages: list[str]
    user_input: str
    result: str
    iteration_count: int

# Create the graph
workflow = StateGraph(GraphState)
```

Subsequently, one can create nodes, which are just Python function that take a state and return an updated state.

```Python
def search_node(state: GraphState) -> GraphState:
    """Search for relevant documents"""
    query = state["user_input"] # separate the user input
    docs = vector_db.search(query) # search vector database
    state["messages"].append(f"Found {len(docs)} documents") # append the resulting docs variable into the general json body
    return state

def llm_node(state: GraphState) -> GraphState:
    """Generate response with LLM"""
    prompt = f"User: {state['user_input']}\nDocs: {state['messages']}" # build the prompt with the new user input and context
    response = llm.invoke(prompt) # feed it to the LLM
    state["result"] = response # update the state/node
    return state

# Add nodes to graph
workflow.add_node("search", search_node) # creation of the first node (search for documents)
workflow.add_node("llm", llm_node) # creation of the second node (llm node for answers)
```

### Edges

There are several types of edges:

1. Normal edges (always go to the next node)

```Python
# Always go from search → llm
workflow.add_edge("search", "llm")
workflow.add_edge("llm", END)  # END is a special terminal node
```

2. Conditional edges (branching logic)

```Python
def should_continue(state: GraphState) -> str:
	"""Decide which node to go to next"""
	if satete["iteration_count"] < 3:
		return "retry"
	else:
		return "finish"

workflow.add_conditional_edges(
	"llm", # From this node
	should_continue, # Decision function
	{
		"retry": "search",  # If returns "retry", go to search
		"finish": END       # If returns "finish", end
	}
)
```

3. Entry point

```Python
workflow.set_entry_point("search") # starts here
```

4. Compiling and running

```Python
app = workflow.compile() # Compile the graph

initial_state = {
    "messages": [],
    "user_input": "What's your refund policy?",
    "result": "",
    "iteration_count": 0
} # Run it

final_state = app.invoke(initial_state)
print(final_state["result"])
```

## When to Use LangGraph vs LangChain

**Use [[LangChain]] when:**

- Simple linear flow (A → B → C)
- No loops or complex branching
- Basic RAG, simple chatbot

**Use LangGraph when:**

- Need loops (retry logic, iterative refinement)
- Conditional branching (if/else logic)
- Multi-agent systems
- Complex state management
- Self-correcting systems

## Takeaways

1. **Nodes** = Functions that process state
2. **Edges** = Control flow (normal, conditional)
3. **State** = Data structure flowing through
4. **Graphs** = Can have cycles (loops), branches, complex logic
5. **Perfect for** = Agents, workflows with retry logic, multi-step reasoning

___
### Sources

1. https://docs.langchain.com/langgraph-platform/langgraph-server