# Claude API Practice

A learning and reference repository for the [Anthropic Claude API](https://docs.anthropic.com/). Each Jupyter notebook is a standalone demo of a distinct API feature — this is not a production application.

## Getting Started

### Prerequisites

- Python 3.x with Jupyter (`jupyter notebook` or `jupyter lab`)
- An Anthropic API key

### Setup

1. Create a `.env` file at the project root (it is gitignored):

   ```
   ANTHROPIC_API_KEY = "sk-ant-..."
   CLAUDE_MODEL = "claude-sonnet-4-6"
   ```

2. Launch Jupyter and open any notebook:

   ```
   jupyter lab
   ```

Each notebook installs its own dependencies via `%pip install` cells (e.g. `anthropic`, `python-dotenv`), so no global install step is required.

Every notebook loads its configuration the same way:

```python
from dotenv import load_dotenv
load_dotenv()
client = anthropic.Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
model = os.getenv("CLAUDE_MODEL")
```

## Notebooks

| Notebook | Feature |
|---|---|
| [`messages_api.ipynb`](messages_api.ipynb) | Basic API calls, streaming, multi-turn conversations |
| [`messages_api_with_helper_functions.ipynb`](messages_api_with_helper_functions.ipynb) | Reusable chat helper patterns |
| [`structured_output.ipynb`](structured_output.ipynb) | JSON Schema and Pydantic with `client.messages.parse()` |
| [`thinking.ipynb`](thinking.ipynb) | Extended thinking with adaptive/medium modes |
| [`reasoning_effort.ipynb`](reasoning_effort.ipynb) | `output_config` reasoning effort control |
| [`messages_with_image.ipynb`](messages_with_image.ipynb) | Vision: URL and base64-encoded images |
| [`user_defined_tool.ipynb`](user_defined_tool.ipynb) | Custom tool schemas and agentic loops |
| [`tool_runner.ipynb`](tool_runner.ipynb) | `client.beta.messages.tool_runner()` with the `@beta_tool` decorator |
| [`tool_search.ipynb`](tool_search.ipynb) | Tool search for context management and dynamic tool discovery |
| [`web_fetch.ipynb`](web_fetch.ipynb) | Built-in web fetch tool |
| [`web_search.ipynb`](web_search.ipynb) | Built-in web search tool |
| [`code_execution.ipynb`](code_execution.ipynb) | Code execution sandbox, file upload/download, matplotlib |
| [`batch_processing.ipynb`](batch_processing.ipynb) | Async batch API for bulk requests |
| [`document_citations.ipynb`](document_citations.ipynb) | Multi-document processing with citation tracking |
| [`memory_tool.ipynb`](memory_tool.ipynb) | `BetaLocalFilesystemMemoryTool` for session persistence |
| [`advisor_tool.ipynb`](advisor_tool.ipynb) | Model-to-model consultation via the advisor beta |

## Project Structure

- **Root notebooks** — the 16 feature demos listed above
- **`Docs/`** — educational text files on tokenization, embeddings, contextualization, and generation, plus sample data (`indian_cricket_team_score.csv`)
- **`Images/`** — sample image assets for the vision demos
- **`memory/`** — runtime storage used by `memory_tool.ipynb` (`base_path="./memory"`); files under `memory/memories/` are written by the model, not hand-authored

## Key Patterns

- **Agentic tool-use loop** (`user_defined_tool.ipynb`): call the API → check for `tool_use` blocks → execute the tool → append the result → loop until the model stops calling tools.
- **Beta features**: accessed via `client.beta.messages.create()` or `client.beta.messages.tool_runner()`, enabled with the `betas=[...]` parameter. Headers currently in use: `advisor-tool-2026-03-01` (`advisor_tool.ipynb`) and `files-api-2025-04-14` (`code_execution.ipynb`, for `client.beta.files.*`).

## Models

Notebooks default to `claude-sonnet-4-6` via the `CLAUDE_MODEL` env var rather than hardcoding model IDs. The one exception is `advisor_tool.ipynb`, which hardcodes `ADVISOR_MODEL = "claude-opus-5"` for the advisor model.
