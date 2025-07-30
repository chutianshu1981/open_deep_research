# Open Deep Research - Project Overview & Architecture

## Project Description
Open Deep Research is a configurable, fully open-source deep research agent built with LangGraph. It automates research workflows by searching multiple sources, analyzing information, and generating comprehensive reports.

## Tech Stack
- **Language**: Python 3.10+
- **Core Framework**: LangGraph for agent orchestration
- **Model Providers**: OpenAI, Anthropic, Google VertexAI, Groq, DeepSeek
- **Search APIs**: Tavily, OpenAI Search, Anthropic Search, DuckDuckGo, Exa
- **Package Management**: UV with pyproject.toml
- **Testing**: pytest
- **Linting**: ruff
- **Type Checking**: mypy

## Core Architecture Components

### Main Entry Points
- `src/open_deep_research/deep_researcher.py` - Main research agent with supervisor-researcher architecture
- `src/open_deep_research/state.py` - Typed state definitions for different agent roles
- `src/open_deep_research/configuration.py` - Pydantic-based configuration system
- `src/open_deep_research/utils.py` - Tool management and MCP integration

### Workflow Stages
1. **Clarification** - Optional user interaction for research scope refinement
2. **Research Brief** - Transform user input into structured research question
3. **Research Supervisor** - Manages research iterations and coordinates sub-agents
4. **Researcher Sub-agents** - Conduct parallel research with tool calling
5. **Compression** - Synthesizes research findings into concise summaries
6. **Final Report** - Generates comprehensive markdown report

### Key Design Patterns
- **Subgraph Architecture**: Supervisor and researcher operate as separate subgraphs
- **Tool-Based Control**: Structured outputs define workflow transitions
- **Concurrent Processing**: Multiple researcher agents work simultaneously
- **Retry Logic**: Automatic retries for structured output failures and token limits

### Legacy Code
- `src/legacy/` contains earlier implementations including workflow and multi-agent versions
- Refer to `src/legacy/CLAUDE.md` for detailed legacy documentation

## File Structure
```
src/
├── open_deep_research/     # Main implementation
│   ├── deep_researcher.py  # Core research agent
│   ├── state.py           # State management
│   ├── configuration.py   # Configuration system
│   ├── utils.py           # Utilities and tool management
│   └── prompts/           # Prompt templates
├── legacy/                # Legacy implementations
└── tests/                 # Test suite
```