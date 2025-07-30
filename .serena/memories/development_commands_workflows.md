# Open Deep Research - Development Commands & Workflows

## Environment Setup
```bash
# Install dependencies
uv pip install -r pyproject.toml

# Set up environment
cp .env.example .env

# Start LangGraph server with Studio UI
uvx --refresh --from "langgraph-cli[inmem]" --with-editable . --python 3.11 langgraph dev --allow-blocking
```

## Code Quality Commands
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

## Testing and Evaluation
- **Quality Evaluation**: Multi-dimensional scoring system (0-1 scale) in `tests/evaluators.py`
- **Batch Evaluation**: Across test datasets with comprehensive criteria
- **Test Criteria**: relevance, structure, citations, quality

### Running Tests
```bash
# Full evaluation suite
python tests/run_evaluate.py

# Test specific research topics
python tests/run_evaluate.py --dataset <dataset_name>
```

## Development Workflow
1. **Setup**: Install dependencies and configure environment
2. **Development**: Write code following existing patterns
3. **Testing**: Run pytest and evaluation suite
4. **Quality**: Run ruff and mypy for code quality
5. **Testing**: Test with LangGraph server locally

## Key Development Practices
- Follow existing code conventions (see ruff configuration in pyproject.toml)
- Use typed state definitions from `state.py`
- Leverage existing configuration patterns from `configuration.py`
- Implement tool-based workflow control using structured outputs
- Use concurrent processing patterns for researcher agents
- Implement retry logic for structured output failures

## Git Workflow
- Main branch: `main`
- Development branch: `feature/dev`
- Commit messages should follow conventional commit format
- Include comprehensive test coverage for new features