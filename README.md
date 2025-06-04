# MCP Quickstart Weather App Extensions

After reading the Official MCP quickstart examples on MCP [server](https://modelcontextprotocol.io/quickstart/server) and [client](https://modelcontextprotocol.io/quickstart/client), do you wonder
- How to upgrade the simple stdio-based example to HTTP server/client towards real-world uses?
  - The [latest MCP document (June 2025)](https://modelcontextprotocol.io/docs/concepts/transports) lists SSE as the default HTTP transport protocol
  - The [latest MCP specification (March 2025)](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports) further upgrades SSE to Streamable HTTP protocol
- How to replace the Anthropic API with OpenAI API widely used in open source inference servers like [vllm](https://docs.vllm.ai/en/latest/)?

## Goal of This Repository

1. Patch the official MCP quickstart weather app to use:
    - SSE or Streamable HTTP as the transport protocol between client and server
    - OpenAI API for LLM calls
2. Explain each modification for readers to understand these extensions

## How to Run

1. Install [uv](https://docs.astral.sh)
2. Choose the protocol in your mind, either `sse` or `streamable-http`
3. Open two terminals on one host (hardcoded localhost HTTP server in this example)
4. Term 1: run server
    - Go to the server directory `weather-server-python`
    - Start the server `uv run server PROTOCOL_OF_YOUR_CHOICE`
5. Term 2: run client
    - Go to the client directory `mcp-client-python`
    - Setup environment variables for OpenAI endpoint and API
        - `export OPENAI_BASE_URL=http://xxx/v1`
        - `export OPENAI_API_KEY=yyy`
    - Start the client `uv run client PROTOCOL_OF_YOUR_CHOICE`

## Explanation of Modifications

### Patch to Use SSE/Streamable-HTTP Instead of Stdio

- Server: use `mcp.run('sys.argv[1]')` instead of `mcp.run('stdio')` given `sys.argv[1]` is either `sse` or `streamable-http`
    - SSE protocol: server main endpoint is `http://localhost:8000/sse`
    - Streamable HTTP protocol: server only endpoint is `http://localhost:8000/mcp`
- Client: load `rs` (readstream), `ws` (writestream) from `sse_client` or `streamablehttp_client` intead of `stdio_client` in the original MCP quickstart example
    - [sse_client awaited return](https://github.com/modelcontextprotocol/python-sdk/blob/main/src/mcp/client/sse.py#L155)
    - [streamablehttp_client awaited return](https://github.com/modelcontextprotocol/python-sdk/blob/main/src/mcp/client/streamable_http.py#L492)

### Swap Anthropic API to OpenAI API for LLM call

- Replace the LLM call function
    - `self.anthropic.messages.create()` -> `self.client.chat.completions.create()`
    - Dynamic model id for vllm
    - The `tools` argument uses a little different formatting
- Replace the LLM response object handling
    - `response` -> `response.choices[0].message`
