# Model Context Protocol (MCP) Integration Guide

Robin now includes support for the Model Context Protocol (MCP), enabling integration with remote servers and enhanced contextual capabilities.

## What is MCP?

The Model Context Protocol (MCP) is an open standard that allows AI models to interact with external systems, data sources, and tools through standardized protocols. It enables:

- **Remote Server Integration**: Connect to external data sources and APIs
- **Tool Calling**: Enable AI models to use external functions and services
- **Context Management**: Maintain stateful workflows across sessions
- **Secure Data Access**: Control and audit tool usage with user consent

## Getting Started

### Installation

MCP support is included when you install Robin's dependencies:

```bash
pip install -r requirements.txt
```

The `mcp` package is automatically installed as part of the requirements.

### Basic Usage

Robin's xAI Grok models support MCP capabilities through the native xAI API integration:

1. **Set up your xAI API key**:
   ```bash
   export XAI_API_KEY=your_xai_api_key_here
   ```

2. **Use Grok models with tool calling**:
   ```bash
   python main.py cli -m grok-beta -q "ransomware payments" -t 12
   ```

### Advanced MCP Server Configuration

To use Robin with custom MCP servers, you can extend the system with additional tools:

#### Example: Custom MCP Server

```python
from mcp import Server, tool

server = Server()

@tool(description="Custom threat intelligence lookup")
def lookup_threat(indicator: str) -> str:
    # Your custom logic here
    return f"Analysis for {indicator}"

server.add_tool(lookup_threat)
server.run()  # Default: stdio transport
```

#### Remote MCP Server

For remote deployments:

```python
server.run_http(host="0.0.0.0", port=8080)
```

## xAI Grok Features

Robin now defaults to using xAI's Grok models which include:

- **grok-beta**: Latest Grok model with reasoning capabilities
- **grok-2-1212**: Stable Grok 2 release
- **grok-2-vision-1212**: Vision-enabled Grok model for multimodal tasks

### Key Features:

1. **Advanced Reasoning**: Enhanced analytical capabilities for threat intelligence
2. **Tool Calling**: Native support for function calling and external tool integration
3. **Streaming Support**: Real-time response streaming for better UX
4. **Context Understanding**: Superior understanding of dark web OSINT context

## Configuration

### Environment Variables

Add to your `.env` file:

```env
# xAI Configuration
XAI_API_KEY=your_xai_api_key

# Optional: OpenRouter for alternative Grok access
OPENROUTER_API_KEY=your_openrouter_key
OPENROUTER_BASE_URL=https://openrouter.ai/api/v1

# Legacy model support (backward compatibility)
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key
GOOGLE_API_KEY=your_google_key
```

## Model Selection Priority

Robin now prioritizes models in this order:

1. **xAI Grok Models** (Native API) - Primary choice
2. **Anthropic Claude Models** - Alternative for specific use cases
3. **Google Gemini Models** - Fast inference option
4. **OpenAI GPT Models** - Kept for backward compatibility
5. **Local Ollama Models** - Privacy-focused local deployment

## Security Considerations

When using MCP with Robin:

- Always use environment variables for API keys
- Review and approve tool executions when prompted
- Use consent-based authorization for sensitive operations
- Audit tool usage logs regularly
- Follow the principle of least privilege for tool access

## Troubleshooting

### Common Issues

**Issue**: `ModuleNotFoundError: No module named 'mcp'`
**Solution**: Run `pip install -r requirements.txt` to ensure all dependencies are installed

**Issue**: Grok model not available
**Solution**: Ensure `XAI_API_KEY` is set in your environment or `.env` file

**Issue**: Tool calling not working
**Solution**: Verify you're using a Grok model (grok-beta, grok-2-1212, etc.)

## Resources

- [MCP Official Documentation](https://modelcontextprotocol.io/)
- [xAI Console](https://console.x.ai/)
- [xAI API Documentation](https://docs.x.ai/)
- [LangChain xAI Integration](https://docs.langchain.com/oss/python/integrations/providers/xai)

## Future Enhancements

Robin's MCP integration roadmap includes:

- [ ] Custom MCP server templates for OSINT workflows
- [ ] Pre-built threat intelligence tool integrations
- [ ] Multi-agent coordination via MCP
- [ ] Enhanced session management and state persistence
- [ ] Integration with popular OSINT APIs via MCP

For questions or contributions, please visit the [Robin GitHub repository](https://github.com/DoubleD710/robinv2).
