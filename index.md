# Log and View Python Based LLM Conversations

**LLM Logger** is a lightweight, local-first tool for inspecting and understanding how your application interacts with large language models like OpenAI GPT-4 or Anthropic Claude.

It helps you:

* Log and inspect each model call with request/response metadata
* View differences between turns in a conversation
* Visualize tool calls, tool responses, and system prompts
* Compare prompt strategies and debug session behavior

Ideal for developers building agent workflows, chat interfaces, or prompt-based systems.

---

## ✨ Features

* ⚡ **One-line setup** – Start logging with a simple wrapper around your OpenAI client  
* 🧠 **Automatic session tracking** – No manual session IDs or state management required  
* 📀 **Local-first logging** – Stores structured logs as JSON on your machine  
* 🔍 **Rich session insights** – Context diffs, tool call/response blocks, and system prompt visibility  
* ⏱️ **Latency + metadata capture** – Track timing, models, and more with every call  
* 🧹 **Framework-agnostic** – Works with any Python codebase  
* 🛡️ **Privacy-first** – Fully offline, no account or server required  
* 🌐 **Simple UI** – Static frontend served locally; no build step needed for end users  
* 👐 **Open source (MIT)** – Lightweight, auditable, and easy to extend  

---

## 🎥 Demo

![LLM Logger Demo](https://private-user-images.githubusercontent.com/5149015/455107230-d4381c74-9541-4bc0-b058-23515be2f866.gif)

---

## 📦 Installation

### 🔹 Installation Options

#### Option 1: From PyPI (Recommended for most users)

```bash
pip install llm-logger
```

#### Option 2: Local Copy (For direct integration or customization)

```bash
git clone https://github.com/akhalsa/llm_debugger.git
cd llm_debugger/llm_logger/front_end
npm install
npx tsc
pip install ./llm_debugger
```

---

### 🔸 Development Setup

```bash
git clone https://github.com/akhalsa/llm_debugger.git
cd llm_debugger
python3 -m venv venv
source venv/bin/activate
pip install -e .
cd llm_logger/front_end
npm install
npx tsc
```

To build and upload to PyPI:

```bash
rm -r dist
python3 -m build
twine upload dist/*
```

---

## 🚀 Usage

### 1. Wrap Your OpenAI Client

```python
from dotenv import load_dotenv
import openai
import os
from llm_logger import wrap_openai

load_dotenv()
api_key = os.getenv("OPENAI_API_KEY")

openai_client = wrap_openai(
    openai.OpenAI(api_key=api_key),
    logging_account_id="my_project"
)
```

Then use `openai_client` as normal:

```python
response = openai_client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What's the capital of France?"}
    ]
)
```

---

### 2. Launch the Log Viewer

**Option A: From Terminal**
```bash
llm_logger
# or with custom port
llm_logger -p 8000
```

Open: http://localhost:8000

**Option B: Inside Your Python Web App**
```python
from fastapi import FastAPI
import uvicorn
from llm_logger.log_viewer import create_log_viewer_app

log_viewer_app = create_log_viewer_app(base_url="/debugger")
app = FastAPI()
app.mount("/debugger", log_viewer_app)
uvicorn.run(app, host="0.0.0.0", port=5000)
```

**Option C: Docker**
```dockerfile
EXPOSE 5000
EXPOSE 8000

CMD bash -c "  uvicorn your_app_module:app --host 0.0.0.0 --port 5000 &   uvicorn llm_logger.log_viewer:app --host 0.0.0.0 --port 8000 && wait"
```

---

## 🛠️ Roadmap Ideas

* Replay conversation with inline visualization  
* Claude and other model support  
* UI analytics and filters  
* Exportable reports and session sharing  
* Plugin hooks and configuration options  

---

## 📬 Feedback

Found a bug or have a feature request? [Open an issue](https://github.com/akhalsa/llm_debugger/issues).

---

## 📜 License

MIT
