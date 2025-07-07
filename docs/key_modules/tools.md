---

title: "Tools"
icon: screwdriver-wrench
------------------------

For more detailed usage information, please refer to our cookbook: [Tools Cookbook](../cookbooks/advanced_features/agents_with_tools.ipynb)

<Card title="What is a Tool?" icon="toolbox">
  A <b>Tool</b> in CAMEL is a callable function with a name, description, input parameters, and an output type.  
  Tools act as the interface between agents and the outside world—think of them like <b>OpenAI Functions</b> you can easily convert, extend, or use directly.
</Card>

<Card title="What is a Toolkit?" icon="screwdriver-wrench">
  A <b>Toolkit</b> is a curated collection of related tools designed to work together for a specific purpose.  
  CAMEL provides a range of built-in toolkits—covering everything from web search and data extraction to code execution, GitHub integration, and much more.
</Card>

## Get Started

<Card title="Install Toolkits" icon="terminal">
To unlock advanced capabilities for your agents, install CAMEL's extra tools package:

<CodeBlock language="sh" title="Install with pip">
pip install 'camel-ai[tools]'
</CodeBlock>

A tool in CAMEL is just a <b>FunctionTool</b>—an interface any agent can call to run custom logic or access APIs. </Card>

<Card title="Define a Custom Tool" icon="toolbox">
You can easily create your own tools for any use case. Just write a Python function and wrap it using <b>FunctionTool</b>:

<CodeBlock language="python" title="add_tool.py">
from camel.toolkits import FunctionTool

def add(a: int, b: int) -> int:
"""Adds two numbers."""
return a + b

add\_tool = FunctionTool(add) </CodeBlock>

Inspect your tool’s properties using built-in methods:

<CodeGroup>
```python tool_properties.py
print(add_tool.get_function_name())
print(add_tool.get_function_description())
print(add_tool.get_openai_function_schema())
print(add_tool.get_openai_tool_schema())
```
```text output.txt
add
Adds two numbers.

{'name': 'add', 'description': 'Adds two numbers.', 'parameters': {'properties': {'a': {'type': 'integer', 'description': 'The first number to be added.'}, 'b': {'type': 'integer', 'description': 'The second number to be added.'}}, 'required': \['a', 'b'], 'type': 'object'}}

{'type': 'function', 'function': {'name': 'add', 'description': 'Adds two numbers.', 'parameters': {'properties': {'a': {'type': 'integer', 'description': 'The first number to be added.'}, 'b': {'type': 'integer', 'description': 'The second number to be added.'}}, 'required': \['a', 'b'], 'type': 'object'}}}

````
</CodeGroup>
</Card>

<Card title="Using Toolkits" icon="screwdriver-wrench">
Toolkits group related tools for specialized tasks—search, math, or automation. Use built-in toolkits or build your own.

<CodeBlock language="python" title="toolkit_usage.py">
from camel.toolkits import SearchToolkit

toolkit = SearchToolkit()
tools = toolkit.get_tools()
</CodeBlock>

You can also wrap toolkit methods as individual FunctionTools:

<CodeBlock language="python" title="custom_tools.py">
from camel.toolkits import FunctionTool, SearchToolkit

google_tool = FunctionTool(SearchToolkit().search_google)
wiki_tool = FunctionTool(SearchToolkit().search_wiki)
</CodeBlock>
</Card>

<Card title="Passing Tools to ChatAgent" icon="user-cog">
You can enhance any <b>ChatAgent</b> with custom or toolkit-powered tools. Just pass the tools during initialization:

<CodeBlock language="python" title="chatagent_tools.py">
from camel.agents import ChatAgent

tool_agent = ChatAgent(
    tools=tools,  # List of FunctionTools
)

response = tool_agent.step("A query related to the tool you added")
</CodeBlock>
</Card>

## Built-in Toolkits

CAMEL provides a variety of built-in toolkits that you can use right away:

| Toolkit | Description |
|---------|-------------|
| ArxivToolkit | Interact with the arXiv API to search and download academic papers. |
| AskNewsToolkit | Fetch news and stories from the AskNews API. |
| AudioAnalysisToolkit | Analyze and transcribe audio content. |
| BrowserToolkit | Browse and extract data from web pages. |
| CodeExecutionToolkit | Execute code in safe environments (Python, Jupyter, Docker, etc.). |
| DalleToolkit | Generate images using OpenAI's DALL-E. |
| DappierToolkit | Fetch real-time data and AI-powered recommendations. |
| DataCommonsToolkit | Query the Data Commons graph for stats and trends. |
| ExcelToolkit | Extract and process Excel files. |
| FileWriteTool | Create or edit text files. |
| FunctionTool | Base for wrapping any Python function as a tool. |
| GitHubToolkit | Access GitHub APIs to fetch issues, PRs, etc. |
| GoogleCalendarToolkit | Create, edit, and delete calendar events. |
| GoogleMapsToolkit | Access Google Maps data. |
| GoogleScholarToolkit | Retrieve publication data from Google Scholar. |
| HumanToolkit | Get user feedback for LLM decisions. |
| ImageAnalysisToolkit | Analyze images with vision-language models. |
| JinaRerankerToolkit | Re-rank documents/images by query relevance. |
| KlavisToolkit | Manage hosted MCP servers on Klavis AI. |
| LinkedInToolkit | Interact with LinkedIn (post, fetch profile). |
| MathToolkit | Perform basic arithmetic calculations. |
| MCPToolkit | Connect to external tools using the MCP protocol. |
| MemoryToolkit | Save and recall agent memory. |
| MeshyToolkit | Perform operations on 3D mesh data. |
| MinerUToolkit | OCR and document analysis using MinerU API. |
| NetworkXToolkit | Work with graph structures using NetworkX. |
| NotionToolkit | Query Notion workspaces and pages. |
| OpenAPIToolkit | Work with OpenAPI/Swagger specs. |
| OpenBBToolkit | Get financial data and market analytics. |
| PPTXToolkit | Create and manipulate PowerPoint slides. |
| PubMedToolkit | Retrieve data from the PubMed API. |
| RedditToolkit | Scrape posts, comments, and analyze trends. |
| RetrievalToolkit | Search a local vector DB. |
| SearchToolkit | Perform web searches via Google, Wiki, etc. |
| SemanticScholarToolkit | Fetch papers and author data via Semantic Scholar. |
| SlackToolkit | Perform Slack operations via API. |
| StripeToolkit | Manage payments and charges via Stripe. |
| SymPyToolkit | Perform symbolic math (e.g., integrals, algebra). |
| TerminalToolkit | Run shell commands and search local files. |
| TwitterToolkit | Manage tweets and profiles via API. |
| VideoAnalysisToolkit | Analyze videos with VLMs. |
| VideoDownloaderToolkit | Download and chunk videos. |
| WeatherToolkit | Fetch weather data via OpenWeatherMap API. |
| WhatsAppToolkit | Send messages via WhatsApp Business API. |
| ZapierToolkit | Trigger Zapier flows from natural language. |

## Using Toolkits as MCP Servers

<Card title="MCP Servers in CAMEL" icon="server">
CAMEL supports the <b>Model Context Protocol (MCP)</b>, letting you expose any toolkit as a standalone server. This enables distributed tool execution and seamless integration across multiple systems.
</Card>

<Card title="What is MCP?" icon="anchor">
MCP is a unified protocol for connecting LLMs with external tools. Any toolkit can be run as an MCP server, enabling remote invocation of tools across environments and languages.
</Card>

<Card title="Expose a Toolkit as an MCP Server" icon="rocket">
You can start any toolkit in server mode. Here’s how to run the ArxivToolkit as an MCP server:

<CodeGroup>
```python
import argparse
import sys
from camel.toolkits import ArxivToolkit

if __name__ == "__main__":
    parser = argparse.ArgumentParser(
        description="Run Arxiv Toolkit with MCP server mode.",
        usage=f"python {sys.argv[0]} [--mode MODE] [--timeout TIMEOUT]",
    )
    parser.add_argument(
        "--mode", choices=["stdio", "sse", "streamable-http"],
        default="stdio", help="MCP server mode (default: 'stdio')"
    )
    parser.add_argument(
        "--timeout", type=float, default=None,
        help="Timeout for the MCP server (default: None)"
    )

    args = parser.parse_args()
    toolkit = ArxivToolkit(timeout=args.timeout)
    toolkit.run_mcp_server(mode=args.mode)
````

</CodeGroup>

Supported modes:

* <b>stdio</b>: Standard input/output (default)
* <b>sse</b>: Server-Sent Events
* <b>streamable-http</b>: Streamable HTTP

  </Card>

<Card title="MCP Server Configuration" icon="settings">
You can configure how toolkits are launched using a config file:

<CodeGroup>
```json mcp_servers_config.json
{
  "mcpServers": {
    "arxiv_toolkit": {
      "command": "python",
      "args": [
        "-m",
        "examples.mcp_arxiv_toolkit.arxiv_toolkit_server",
        "--timeout",
        "30"
      ]
    }
  }
}
```
</CodeGroup>
</Card>

<Card title="Connect to MCP Servers as a Client" icon="plug">
Client apps can connect to and invoke tools from MCP servers:

<CodeGroup>
```python mcp_client_example.py
import asyncio
from mcp.types import CallToolResult
from camel.toolkits.mcp_toolkit import MCPToolkit, MCPClient

async def run\_example():
mcp\_toolkit = MCPToolkit(config\_path="path/to/mcp\_servers\_config.json")
await mcp\_toolkit.connect()

```
mcp_client: MCPClient = mcp_toolkit.servers[0]

res = await mcp_client.list_mcp_tools()
if isinstance(res, str):
    raise Exception(res)
tools = [tool.name for tool in res.tools]
print(f"Available tools: {tools}")

result: CallToolResult = await mcp_client.session.call_tool(
    "tool_name", {"param1": "value1", "param2": "value2"}
)
print(result.content[0].text)

await mcp_toolkit.disconnect()
```

if **name** == "**main**":
asyncio.run(run\_example())

```
</CodeGroup>
</Card>

<Card title="Benefits of MCP Servers" icon="zap">
<ul>
  <li><b>Distributed Execution:</b> Run tools anywhere—across machines or containers.</li>
  <li><b>Process Isolation:</b> Each toolkit runs in its own process for stability and security.</li>
  <li><b>Resource Management:</b> Allocate memory/CPU for heavier toolkits independently.</li>
  <li><b>Scalability:</b> Easily scale individual toolkits.</li>
  <li><b>Language Interoperability:</b> MCP works across Python, JS, and more.</li>
</ul>
</Card>

<Card title="Best Practices for MCP Integration" icon="star">
<ul>
  <li><b>Set timeouts</b> to prevent tool hang-ups.</li>
  <li><b>Handle errors</b> gracefully in both server and client logic.</li>
  <li><b>Clean up resources</b> when finished using a toolkit.</li>
  <li><b>Use environment variables</b> or config files for flexible deployment.</li>
  <li><b>Log and monitor</b> toolkit performance in production.</li>
</ul>
</Card>

<Card title="Conclusion" icon="trophy">
Tools—especially when deployed as MCP servers—are the bridge between CAMEL agents and the real world. With this architecture, you can empower agents to automate, fetch, compute, and integrate with almost any external system.
</Card>

```
