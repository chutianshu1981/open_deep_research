# Open Deep Research - Configuration & Environment Setup

## Environment Variables
### Model Configuration
- `RESEARCH_MODEL` - Model for research tasks
- `COMPRESSION_MODEL` - Model for compression tasks  
- `FINAL_REPORT_MODEL` - Model for final report generation

### Search API Configuration
- `SEARCH_API` - Search provider (tavily, openai, anthropic, none)

### MCP Configuration
- `MCP_CONFIG` - JSON configuration for MCP servers

## Model Requirements
- All models must support structured outputs and tool calling
- Search API compatibility requirements:
  - Anthropic models for Anthropic search
  - OpenAI models for OpenAI search
  - Tavily works with all model providers

## Configuration System
- **Pydantic-based**: Type-safe configuration with validation
- **Environment Variable Support**: Configurable fallbacks
- **Model Provider Abstraction**: Uses `init_chat_model()` for provider flexibility
- **UI Metadata**: Integrated with Open Agent Platform

## MCP Integration
### Supported Features
- **Local MCP Servers**: Direct server integration
- **Remote MCP Servers**: JWT authentication support
- **Filesystem MCP**: Secure file operations
- **Error Handling**: Robust error management for MCP operations

### Configuration Pattern
```python
# MCP servers are configured via MCP_CONFIG JSON
# Supports multiple simultaneous MCP servers
# Includes authentication and connection parameters
```

## Environment Setup Process
1. **Copy Environment Template**: `cp .env.example .env`
2. **Configure Models**: Set appropriate model variables
3. **Configure Search API**: Choose search provider
4. **Configure MCP**: Set up MCP servers if needed
5. **Install Dependencies**: `uv pip install -r pyproject.toml`

## Key Configuration Files
- `.env.example` - Environment variable template
- `src/open_deep_research/configuration.py` - Main configuration system
- `langgraph.json` - LangGraph server configuration
- `pyproject.toml` - Project dependencies and tool configuration