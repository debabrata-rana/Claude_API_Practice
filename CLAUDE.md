# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This is a learning and reference repository for the Anthropic Claude API. It demonstrates API features through standalone Jupyter notebooks, not a production application.

## Environment Setup

Requires a `.env` file at the project root:
```
ANTHROPIC_API_KEY = "sk-ant-..."
CLAUDE_MODEL = "claude-sonnet-4-6"
```

Every notebook loads it the same way and reads the model from the env var rather than hardcoding it. When adding a notebook, follow this pattern:
```python
from dotenv import load_dotenv
load_dotenv()
client = anthropic.Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
model = os.getenv("CLAUDE_MODEL")
```

`.env` is gitignored.

## Running Code

- **Notebooks**: `jupyter notebook` or `jupyter lab`, then open any `.ipynb` file
- **Install deps**: Each notebook installs its own deps via `%pip install anthropic python-dotenv ...`

No build step, test runner, or linting setup exists in this repo.

## Architecture

### Structure
- **Root notebooks** (16 `.ipynb` files) — each demonstrates a distinct API feature
- **`Docs/`** — educational text files explaining tokenization, embeddings, etc.
- **`Images/`** — sample image assets for vision capability demos
- **`memory/`** — runtime storage for `memory_tool.ipynb` (`base_path="./memory"`); files under `memory/memories/` are written by the model, not hand-authored

### Feature Coverage by Notebook

| Notebook | Feature |
|---|---|
| `messages_api.ipynb` | Basic API calls, streaming, multi-turn conversations |
| `messages_api_with_helper_functions.ipynb` | Reusable chat helper patterns |
| `structured_output.ipynb` | JSON Schema and Pydantic with `client.messages.parse()` |
| `thinking.ipynb` | Extended thinking with adaptive/medium modes |
| `reasoning_effort.ipynb` | `output_config` reasoning effort control |
| `messages_with_image.ipynb` | Vision: URL and base64-encoded images |
| `user_defined_tool.ipynb` | Custom tool schemas and agentic loops |
| `tool_runner.ipynb` | `client.beta.messages.tool_runner()` with `@beta_tool` decorator |
| `tool_search.ipynb` | Tool search for context management and dynamic tool discovery |
| `web_fetch.ipynb` | Built-in web fetch tool |
| `web_search.ipynb` | Built-in web search tool |
| `code_execution.ipynb` | Code execution sandbox, file upload/download, matplotlib |
| `batch_processing.ipynb` | Async batch API for bulk requests |
| `document_citations.ipynb` | Multi-document processing with citation tracking |
| `memory_tool.ipynb` | `BetaLocalFilesystemMemoryTool` for session persistence |
| `advisor_tool.ipynb` | Model-to-model consultation via advisor beta |

### Key Patterns

**Agentic tool-use loop** (in `user_defined_tool.ipynb`): call API → check for `tool_use` blocks → execute tool → append result → loop until no more tool calls.

**Beta features**: accessed via `client.beta.messages.create()` or `client.beta.messages.tool_runner()`; enabled via `betas=[...]` parameter. Headers currently in use: `advisor-tool-2026-03-01` (`advisor_tool.ipynb`) and `files-api-2025-04-14` (`code_execution.ipynb`, for `client.beta.files.*`).

## Current Models

Default to `claude-sonnet-4-6` (via `CLAUDE_MODEL`). Exception: `advisor_tool.ipynb` hardcodes `ADVISOR_MODEL = "claude-opus-5"` for the advisor. Available IDs: `claude-opus-4-7`, `claude-sonnet-4-6`, `claude-haiku-4-5-20251001`.
