# Source code

```Python
import os

from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, END
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, AIMessage
from langsmith import Client
from dotenv import load_dotenv

load_dotenv()

os.environ["OPENAI_API_KEY"]
os.environ["LANGCHAIN_TRACING_V2"]
os.environ["LANGSMITH_API_KEY"]

client = Client()

class AgentState(TypedDict):
	messages: list
	next_step: str
	iteration: int

llm = ChatOpenAI(model="gpt-4", temperature=0.7)

def process_input(state: AgentState) -> AgentState:
"""process user input and determine next action"""
	messages = state["messages"]
	response = llm.invoke(messages)
	state["messages"].append(response)
	state["iteration"] += 1
	
	if state["iteration"] >= 3:
		state["next_step"] = "end"
	else:
		state["next_step"] = "continue"
	return state

  

def refine_response(state: AgentState) -> AgentState:
	"""refine the responses if necessary"""
	messages = state["messages"]
	refinement_msg = HumanMessage(content="Please make this response more concise")
	response = llm.invoke(messages + [refinement_msg])
	state["messages"].append(response)
	return state

  

def create_graph():
	"""LangGraph workflow goes here"""
	workflow = StateGraph(AgentState)
	workflow.add_node("process", process_input)
	workflow.add_node("refine", refine_response)
	# edges
	workflow.set_entry_point("process")
	def should_continue(state: AgentState) -> str:
		if state["next_step"] == "end":
			return "end"
		return "refine"
	workflow.add_conditional_edges(
	"process",
	should_continue,
		{
			"refine": "refine",
			"end": END
		}
	)
	
	workflow.add_edge("refine", END)
	return workflow.compile()

  

def run_agent(user_input: str):
	"""main execution with langsmith"""
	initial_state = {
	"messages": [HumanMessage(content=user_input)],
	"next_step": "continue",
	"iteration": 0
	} 
	graph = create_graph()
	result = graph.invoke(initial_state)
	return result

  

if __name__ == "__main__":
while True:
	prompt = input("Enter prompt: ")
	if prompt == "exit":
		break
	result = run_agent(prompt)
	print("Final conversation:")
	for msg in result["messages"]:
		role = "Human" if isinstance(msg, HumanMessage) else "AI"
		print(f"\n{role}: {msg.content}")
	print(f"\n\nTotal iterations: {result['iteration']}")
	print("View traces in LangSmith dashboard: https://smith.langchain.com/")

```