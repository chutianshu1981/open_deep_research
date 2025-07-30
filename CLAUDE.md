# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Open Deep Research is a configurable, fully open-source deep research agent built with LangGraph. It automates research workflows by searching multiple sources, analyzing information, and generating comprehensive reports. The system supports multiple model providers, search APIs, and MCP servers for extensibility.

## Development Commands

### Environment Setup
```bash
# Install dependencies
uv pip install -r pyproject.toml

# Set up environment
cp .env.example .env

# Start LangGraph server with Studio UI
uvx --refresh --from "langgraph-cli[inmem]" --with-editable . --python 3.11 langgraph dev --allow-blocking
```

### Code Quality
```bash
# Run linting
ruff check .

# Type checking
mypy .

# Run tests
pytest

# Run evaluation suite
python tests/run_evaluate.py
```

## Architecture

### Core Components

**Main Research Agent** (`src/open_deep_research/deep_researcher.py`):
- Multi-stage research workflow with supervisor-researcher architecture
- Supports concurrent research units for parallel processing
- Configurable models for different tasks (summarization, research, compression, final report)

**State Management** (`src/open_deep_research/state.py`):
- Typed state definitions for different agent roles
- Structured outputs for tool calling and workflow control
- Override reducers for state management in subgraphs

**Configuration System** (`src/open_deep_research/configuration.py`):
- Pydantic-based configuration with UI metadata for Open Agent Platform
- Environment variable support with configurable fallbacks
- Model provider abstraction using `init_chat_model()`

**Utilities** (`src/open_deep_research/utils.py`):
- Tool management for search APIs (Tavily, OpenAI, Anthropic)
- MCP server integration with error handling
- Token limit management and retry logic

### Workflow Stages

1. **Clarification**: Optional user interaction for research scope refinement
2. **Research Brief**: Transform user input into structured research question
3. **Research Supervisor**: Manages research iterations and coordinates sub-agents
4. **Researcher Sub-agents**: Conduct parallel research with tool calling
5. **Compression**: Synthesizes research findings into concise summaries
6. **Final Report**: Generates comprehensive markdown report

### Key Design Patterns

- **Subgraph Architecture**: Supervisor and researcher operate as separate subgraphs
- **Tool-Based Control**: Structured outputs define workflow transitions
- **Concurrent Processing**: Multiple researcher agents work simultaneously
- **Retry Logic**: Automatic retries for structured output failures and token limits

## Configuration

### Model Requirements
- All models must support structured outputs and tool calling
- Search API compatibility: Anthropic models for Anthropic search, OpenAI models for OpenAI search
- Tavily works with all model providers

### Environment Variables
- Model configurations: `RESEARCH_MODEL`, `COMPRESSION_MODEL`, `FINAL_REPORT_MODEL`
- Search API: `SEARCH_API` (tavily, openai, anthropic, none)
- MCP server configuration via `MCP_CONFIG` JSON

### MCP Integration
- Supports both local and remote MCP servers
- Filesystem MCP for secure file operations
- Remote MCP servers with JWT authentication support

## Testing and Evaluation

### Quality Evaluation
- Multi-dimensional scoring system (0-1 scale) in `tests/evaluators.py`
- Batch evaluation across test datasets
- Comprehensive criteria: relevance, structure, citations, quality

### Running Tests
```bash
# Full evaluation suite
python tests/run_evaluate.py

# Test specific research topics
python tests/run_evaluate.py --dataset <dataset_name>
```

## Legacy Code

The `src/legacy/` folder contains earlier implementations:
- **Workflow Implementation**: Plan-and-execute with human-in-the-loop
- **Multi-Agent Implementation**: Alternative parallel processing approach

See `src/legacy/CLAUDE.md` for detailed documentation of legacy systems.