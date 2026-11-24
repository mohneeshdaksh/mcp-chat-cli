# MCP Chat CLI

MCP Chat is a command-line interface application that enables interactive chat capabilities with AI models through the Anthropic API. The application supports document retrieval, command-based prompts, and extensible tool integrations via the MCP (Model Context Protocol) architecture.

## Prerequisites

- Python 3.10 to 3.13
- Anthropic API Key

## Setup

### Step 1: Configure the environment variables

1. Create or edit the `.env` file in the project root and verify that the following variables are set correctly:

```
ANTHROPIC_API_KEY=""             # Your Anthropic API key
CLAUDE_MODEL="claude-haiku-5-5"  # Claude model to use
USE_UV=1                         # 1 if using uv, 0 if using pip
```

### Step 2: Install dependencies

#### Option 1: Setup with uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a fast Python package installer and resolver.

1. Install uv, if not already installed:

```bash
pip install uv
```

2. Create and activate a virtual environment:

```bash
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies:

```bash
uv pip install -e .
```

4. Run the project

```bash
uv run main.py
```

#### Option 2: Setup without uv

1. Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

2. Install dependencies, and set `USE_UV=0` in the `.env` file:

```bash
pip install "anthropic<1" python-dotenv prompt-toolkit "mcp[cli]>=1.8.0,<2" "certifi==2026.10.18"
```

3. Run the project

```bash
python main.py
```

## Usage

### Basic Interaction

Simply type your message and press Enter to chat with the model.
Press Ctrl+C to exit.

### Document Retrieval

Use the @ symbol followed by a document ID to include document content in your query:

```
> Tell me about @deposition.md
```

### Commands

Use the / prefix to execute commands defined in the MCP server:

```
> /format deposition.md
```

Commands will auto-complete when you press Tab.

## Development

### Adding New Documents

Edit the `mcp_server.py` file to add new documents to the `docs` dictionary.

## What's Implemented

**MCP Server** (`mcp_server.py`)
- **Tools:** `read_doc_contents`, `edit_document`. Claude calls these to read and edit documents.
- **Resources:** `docs://documents` (list of document IDs) and `docs://documents/{doc_id}` (document contents). They power `@` mentions.
- **Prompts:** `format` (rewrites a document in Markdown). Available as `/format`.

**MCP Client** (`mcp_client.py`)
- Connects to the server over stdio and implements `list_tools`, `call_tool`, `read_resource`, `list_prompts` and `get_prompt`.

### Linting and Typing Check

There are no lint or type checks implemented.

## Acknowledgements

This project started from the starter code provided in Anthropic Academy's
[Introduction to Model Context Protocol](https://academy.claude.com/courses/introduction-to-model-context-protocol/)
course. The original starter code belongs to Anthropic. The MCP server and
client implementations, and any later features, are my own work.