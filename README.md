# Virtual Study Planner 

This AI agent will be able to understand your goals, make decisions, and act to achieve them.

Memory: The agent will remember your entire conversation history, allowing it to provide follow-up advice and adapt its plans based on your feedback.

Tool Use: The agent will be capable of using a search tool to find relevant online resources, making it a more powerful assistant than one that relies solely on its internal knowledge.

## Understanding AI Agents
An AI agent is software that can autonomously perform tasks on a user’s behalf. AI agents perceive their surroundings, process information, and act to achieve the user’s goals. Unlike fixed programs, an agent can reason and adapt.

There are a few different types of agents, including:

Simple Reflex (acts on current input, like a thermostat)
Model-Based (uses an internal map, like robot vacuums)
Goal-Based (plans to reach goals, like a study planner)
Utility-Based (chooses best outcomes, like trading bots)
Learning Agents (improve over time, like recommendation systems).

AI agents use technologies like LLMs, but they’re distinct because of their autonomy and ability to act. 

There are 3 different types of AI Tools
Large Language Models (LLMs): LLMs are the brain of the operation. They’re trained on a very large dataset to understand and process user queries in natural language to generate human-like output. OpenAI’s GPT, Google’s Gemini, and Anthropic’s Claude are all examples of LLMs.

Retrieval-Augmented Generation (RAG): RAG is a process or a technique that allows LLMs to not only get their information from training data but also from external sources, like a database or document library, to answer user queries. While RAG retrieves information, it doesn't independently decide to perform an action or plan a sequence of steps to achieve a goal.

AI Agents: As explained above, agents are the systems that can perform user tasks using LLMs as their core reasoning engine. An agent’s full architecture allows it to perceive its environment, plan, act, and learn (memory, based on past interactions).

### In this tutorial, you are going to use an LLM (Gemini) to reason, as well as a web search engine, DuckDuckGo search, for building the agent. So, now let’s move on to the next step.

### How to Build the Real-Time Agent Logic
The core of this project is a continuous loop that accepts user input, maintains a conversation history, and sends that history to the Gemini API to generate a response. This is how we give the agent memory.

### In Gemini-Client file
#### The perform_web_search() function:

We keep a chat session open so the model remembers the conversation.

If a message starts with search: or /search, the DuckDuckGo service is called, gathers a few results, and passes them to Gemini with a short instruction to cite sources.

Otherwise, we just send the message as normal.

#### The GeminiClient class:

The GeminiClient class is designed to connect and talk with Google’s Gemini AI. Inside the __init__ method, it first calls genai.configure() with the API key from the environment variables, which basically unlocks access to Gemini’s services.

Then, self.model = genai.GenerativeModel('gemini-1.5-flash') loads the specific Gemini model, and self.chat = self.model.start_chat(history=[]) starts a new conversation with no previous history. This way, the class is ready to send and receive AI responses.

The real action happens in generate_response(). If a user’s message begins with search: or /search, it triggers a DuckDuckGo search using perform_web_search().

The results are formatted with titles, links, and snippets, and then passed to Gemini to create a clear, cited answer (you can sanitize the incoming data later by using any package in Python to make it more user-friendly in the frontend).

If no search command is used, it simply chats with Gemini using the given input. Error handling is built in, so instead of breaking, it returns a general safe message.

#### The Flask web server to connect our agent logic to a simple web interface.

### In App.py

@app.route('/'): This is the homepage. When a user navigates to the main URL, like, http://localhost:5000), Flask runs the index() function, which simply renders the index.html file. This serves the entire user interface to the browser useful when you don’t want to use the command line interface.

Next, we have created @app.route('/api/chat', methods=['POST']), the API endpoint. When the user clicks "Send" on the frontend, the JavaScript sends a POST request to this URL. The chat() function then receives the user's message, passes it to the GeminiClient to get a response, and then sends that response back to the frontend as a JSON object.

All important dependencies have been intalled. 
