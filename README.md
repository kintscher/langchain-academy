![LangChain Academy](https://cdn.prod.website-files.com/65b8cd72835ceeacd4449a53/66e9eba1020525eea7873f96_LCA-big-green%20(2).svg)

## Introduction

Welcome to LangChain Academy, Introduction to LangGraph! 
This is a growing set of modules focused on foundational concepts within the LangChain ecosystem. 
Module 0 is basic setup and Modules 1 - 5 focus on building in LangGraph, progressively adding more advanced themes.  Module 6 addresses deploying your agents. 
In each module folder, you'll see a set of notebooks. A link to the LangChain Academy lesson is at the top of each notebook to guide you through the topic. Each module also has a `studio` subdirectory, with a set of relevant graphs that we will explore using the LangGraph API and Studio.

## Setup

This fork uses [uv](https://docs.astral.sh/uv/) to manage the Python environment and dependencies. If you don't have uv installed yet, see the [uv install instructions](https://docs.astral.sh/uv/getting-started/installation/) (e.g., `curl -LsSf https://astral.sh/uv/install.sh | sh` on macOS/Linux, or `winget install --id=astral-sh.uv` on Windows).

### Python version

The course supports Python 3.11, 3.12, or 3.13. uv can install a matching interpreter for you — no system Python required:
```
uv python install 3.12
```

### Clone repo
```
git clone https://github.com/langchain-ai/langchain-academy.git
cd langchain-academy
```
Or, if you prefer, you can download a zip file [here](https://github.com/langchain-ai/langchain-academy/archive/refs/heads/main.zip).

### Create an environment and install dependencies
#### Mac/Linux/WSL
```
$ uv venv --python 3.12 .venv
$ source .venv/bin/activate
$ uv pip install -r requirements.txt
```
#### Windows Powershell
```
PS> uv venv --python 3.12 .venv
PS> .\.venv\Scripts\Activate.ps1
PS> uv pip install -r requirements.txt
```

### Running notebooks
Jupyter is included in `requirements.txt`, so it's already in your `.venv`. Launch it with uv (no separate install needed):
```
$ uv run jupyter notebook
```
You can also use `uv run` to invoke any other CLI from this environment (e.g., `uv run langgraph dev`) without manually activating the venv.

### Setting up env variables

This fork uses a single `.env` file at the repo root. A template is provided in [`.env.template`](.env.template) — copy it and fill in your keys:

```
cp .env.template .env
```

Then open `.env` in your editor and replace the placeholder values. `.env` is listed in `.gitignore`, so your real keys will never be committed.

The notebooks read variables from the environment with `os.environ` (and prompt interactively via `getpass` if a key is missing). If you prefer, you can also export them in your shell:

#### Mac/Linux/WSL
```
$ export ANTHROPIC_API_KEY="your-api-key-here"
```
#### Windows Powershell
```
PS> $env:ANTHROPIC_API_KEY = "your-api-key-here"
```

### Set Anthropic API key (required)
* This fork uses Anthropic's Claude models (`claude-sonnet-4-6` by default) via the [`langchain-anthropic`](https://docs.langchain.com/oss/python/integrations/chat/anthropic) integration.
* If you don't have an Anthropic API key, you can sign up [here](https://console.anthropic.com/).
* Set `ANTHROPIC_API_KEY` in your `.env` file.

### Sign up and Set LangSmith API (recommended)
* Sign up for LangSmith [here](https://docs.langchain.com/langsmith/create-account-api-key#create-an-account-and-api-key); find out more about LangSmith and how to use it within your workflow [here](https://www.langchain.com/langsmith).
* Set `LANGSMITH_API_KEY`, `LANGSMITH_TRACING_V2="true"`, `LANGSMITH_PROJECT="langchain-academy"` in your `.env` file.
* If you are on the EU instance also set `LANGSMITH_ENDPOINT="https://eu.api.smith.langchain.com"`.

### Set up Tavily API for web search (optional)

* Tavily Search API is a search engine optimized for LLMs and RAG, aimed at efficient, quick, and persistent search results.
* You can sign up for an API key [here](https://tavily.com/). It's easy to sign up and offers a very generous free tier. Some lessons (in Module 4) will use Tavily.
* Set `TAVILY_API_KEY` in your `.env` file.

### Set up Studio

* Studio is a custom IDE for viewing and testing agents.
* Studio can be run locally and opened in your browser on Mac, Windows, and Linux.
* See documentation [here](https://docs.langchain.com/langsmith/studio#local-development-server) on the local Studio development server. 
* Graphs for LangGraph Studio are in the `module-x/studio/` folders for module 1-5.
* To start the local development server, make sure your virtual environment is active and run the following command in your terminal in the `/studio` directory in each module:

```
uv run langgraph dev
```
(Or `langgraph dev` directly if the `.venv` is already activated.)

You should see the following output:
```
- 🚀 API: http://127.0.0.1:2024
- 🎨 Studio UI: https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024
- 📚 API Docs: http://127.0.0.1:2024/docs
```

Open your browser and navigate to the Studio UI: `https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024`.

* To use Studio, you will need a `.env` file inside each `module-x/studio/` directory with the relevant API keys.
* Run this from the command line to create those files for modules 1 to 5 from your root `.env` (assumes `ANTHROPIC_API_KEY` and `TAVILY_API_KEY` are exported in your shell — e.g., `set -a; source .env; set +a`):
```
for i in {1..5}; do
  cp module-$i/studio/.env.example module-$i/studio/.env
  echo "ANTHROPIC_API_KEY=\"$ANTHROPIC_API_KEY\"" > module-$i/studio/.env
done
echo "TAVILY_API_KEY=\"$TAVILY_API_KEY\"" >> module-4/studio/.env
```
