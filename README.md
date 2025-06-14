# Weather App: MCP Server and Conversational Client

This project showcases a weather application that leverages a custom **MCP server** built using the **uv** Python package manager, and a conversational AI client powered by **Gemini LLM**. The server fetches weather data for U.S. states, while the client allows natural, interactive conversations with users.


> 💡 New to MCP? [Start here with the official MCP Documentation](https://modelcontextprotocol.io/introduction) — it's a great intro to the protocol this project builds on.


---

## ✨ Chat Example

### Terminal Interaction Between Client, Server, and Gemini AI

Upon running both the client and server, here's an example session:

```
weatheruser@hp:~/Desktop/new work/mcp practice/weather$ uv run client/client.py server/weather.py  
[06/14/25 14:24:01] INFO     Processing request of type ListToolsRequest                                                                                                               server.py:523

Connected to server with tools: ['get_alerts', 'get_forecast']

MCP Client Started!
Type your queries or 'quit' to exit.

Query: hi

Hi there! How can I help you with the weather today?


Query: tell me about the weather in california

[Gemini requested tool call: get_alerts with args {'state': 'CA'}]
[06/14/25 14:25:19] INFO     Processing request of type CallToolRequest                                                                                                                server.py:523
                    INFO     HTTP Request: GET https://api.weather.gov/alerts/active/area/CA "HTTP/1.1 200 OK"                                                                       _client.py:1740

OK. California has a few alerts: There's a beach hazards statement for much of the coast, and an extreme heat warning for the southeast desert and valley areas. Be careful if you're near the water or in the desert.&#x20;


Query: how about in seattle

I do not have the ability to search for weather alerts in specific cities, I can only search by state. Seattle is in Washington, would you like me to search for alerts there?


Query: give me the weather alerts of the state where seattle is

[Gemini requested tool call: get_alerts with args {'state': 'WA'}]
[06/14/25 14:26:14] INFO     Processing request of type CallToolRequest                                                                                                                server.py:523
[06/14/25 14:26:15] INFO     HTTP Request: GET https://api.weather.gov/alerts/active/area/WA "HTTP/1.1 200 OK"                                                                       _client.py:1740

There are no active weather alerts for the state of Washington.


```

```
Query: can you tell me the weather forecast in california

I can give you a weather forecast, but I need a specific latitude and longitude for the location in California that you''re interested in.


Query: find the latitude and longitude of UCLA and give me the forcast

[Gemini requested tool call: get_forecast with args {'longitude': -118.4451, 'latitude': 34.0689}]
[06/14/25 14:28:10] INFO     Processing request of type CallToolRequest                                                                                                                server.py:523
[06/14/25 14:28:11] INFO     HTTP Request: GET https://api.weather.gov/points/34.0689,-118.4451 "HTTP/1.1 200 OK"                                                                    _client.py:1740
[06/14/25 14:28:12] INFO     HTTP Request: GET https://api.weather.gov/gridpoints/LOX/148,47/forecast "HTTP/1.1 200 OK"                                                              _client.py:1740

Here's the weather forecast for UCLA:

Overnight: Patchy fog. Cloudy, with a low around 61°F. South wind around 0 mph.

Saturday: Patchy fog before 11am. Mostly sunny, with a high near 74°F. South wind 0 to 10 mph.

Saturday Night: Patchy fog after 11pm. Mostly cloudy, with a low around 62°F. Southeast wind 0 to 5 mph.

Sunday: Patchy fog before 11am. Mostly sunny, with a high near 80°F. South southwest wind 0 to 10 mph.

Sunday Night: Patchy fog after 11pm. Mostly cloudy, with a low around 62°F. Southeast wind 5 to 10 mph.


Query: quit
weatheruser@hp:~/Desktop/new work/mcp practice/weather$ 


```

---

## 🛠️ Prerequisites & Tools

- **Python 3.10+**
- [**uv**](https://github.com/astral-sh/uv): Fast alternative to pip/poetry for Python environment and dependency management
- **uv-mcp**: MCP protocol server tooling
- **Gemini LLM**: For conversational interaction and function-calling (tool use)
- **U.S. Weather API**: Only supports U.S. states
- Optional:
  - `uvx` alias for running CLI tools via uv
  - MCP Inspector, Cursor, Claude desktop, or any custom client

---

## ⚙️ Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/KMobin555/weather.git
cd weather
```

### 2. Install Dependencies Using `uv`

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh   # install uv
uv init .                                         # initialize uv project
uv add uv-mcp                                     # add MCP server tool
uv sync                                           # install all dependencies
```

If you're falling back to pip:

```bash
uv pip install -r requirements.txt
```

### 3. Configure API Keys

Create a `.env` file in the project root:

```
GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"
```

---

## 🚀 Running the App

### Option 1: Run Both Server and Client Together

This runs an interactive terminal app where both the server and Gemini-based client run in tandem:

```bash
uv run client/client.py server/weather.py
```

Your terminal will now act as a conversational interface. Ideal for testing, development, and CLI-based interactions.

### Option 2: Run Server Only (For External Clients)

Use this when you want to connect via a different client like **MCP Inspector**, **Claude desktop**, or a custom Streamlit app:

```bash
uv run mcp dev server/weather.py
```

If MCP Inspector is not installed, you'll be prompted to install it. Once running, you can open [https://127.0.0.1:6274](https://127.0.0.1:6274) and connect to your local MCP server.

To change the port, pass the `--port` flag or configure via `.env`.

---

## 📊 Features

- Conversational weather querying with Gemini LLM
- Tool use via `get_alerts` and `get_forecast`
- State-level alert coverage
- Forecast support with lat/lon
- Custom MCP protocol server with uv

---

## 📚 License

This project is open-source and available under the MIT License.

---

Enjoy using your AI-powered weather assistant for U.S. states, crafted with uv and Gemini LLM! ☀️🌧️
