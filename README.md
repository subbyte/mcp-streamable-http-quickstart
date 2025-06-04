# MCP Quickstart Weather App Extensions

After reading the Official MCP quickstart examples on MCP [server](https://modelcontextprotocol.io/quickstart/server) and [client](https://modelcontextprotocol.io/quickstart/client), do you wonder
- How to upgrade the simple stdio-based example to HTTP server/client towards real-world uses?
  - The [latest MCP document (June 2025)](https://modelcontextprotocol.io/docs/concepts/transports) lists SSE as the default HTTP transport protocol
  - The [latest MCP specification (May 2025)](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports) further upgrades SSE to Streamable HTTP protocol
- How to replace the Anthropic API with OpenAI API widely used in open source inference servers like [vllm](https://docs.vllm.ai/en/latest/)?

### Goal of This Repository

1. Patch the official MCP quickstart weather app to use:
    - OpenAI API for LLM calls
    - SSE or Streamable HTTP as the transport protocol between client and server
2. Explain each modification for readers to go beyond these extensions
