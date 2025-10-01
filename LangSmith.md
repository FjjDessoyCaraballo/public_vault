# What is LangSmith

While [[LangChain]] and [[LangGraph]] are mainly doing execution of the user input and respones, there is a lot going on in the background that can go by unnoticed. That's when LangSmith comes into places with observability and debugging for LLM applications. LangSmith aims to solve the issue of the complex probabilistic nature of LLMs, so one can have visibility on what's happening at each step.

## Core features

1. Tracing
	1. every LLM call
	2. input and output at each step
	3. tokens used and latency
	4. tool calls
	5. errors
2. Debugging
	1. which prompt produced a bad response;
	2. what data was retrieved from your vector database
	3. what errors occurred;
	4. where errors occurred;
	5. token counts and costs;
3. Testing & Evaluation
	1. create datasets of test inputs/outputs
	2. run your chain against them
	3. compare results
	4. measure accuracy
4. Monitoring (production)
5. Prompt Management