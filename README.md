# 🎤 Agentic Voice Coder: Autonomous Stateful Orchestration & Code Mutation

<img width="1306" height="696" alt="Screenshot 2026-05-27 221817" src="https://github.com/user-attachments/assets/8ec2d9d4-d2d4-47a5-8fc4-e090e836157b" />

---

This project addresses the complexities of building a localized, voice-activated Agent. It solves two critical challenges in autonomous coding workflows: **Multi-modal State Orchestration (Voice-to-Code)** and **Token-Efficient Codebase Mutation (Deterministic Diffing)**.

![Python](https://img.shields.io/badge/Python-3.12-blue.svg)
![LangGraph](https://img.shields.io/badge/LangGraph-State_Orchestration-orange.svg)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-green.svg)
![Jupyter](https://img.shields.io/badge/Jupyter-ipywidgets-critical.svg)
![Status](https://img.shields.io/badge/Status-Completed-success.svg)


## 🎯 Project Overview

This notebook implements a interactive coding assistant entirely within a Jupyter environment. The system translates raw human speech into actionable developer intent, executing surgical code edits while maintaining a highly responsive, non-blocking Graphical User Interface.

Key engineering achievements include:
1. **Asynchronous Voice Modality:** A multi-threaded `pyaudio` ingestion engine that prevents Jupyter kernel lockups.
2. **Cyclic Reasoning Engine:** A `LangGraph` state machine that intelligently routes between writing net-new code and patching existing logic.
3. **Advanced IDE UI:** A symmetrical, Pygments-rendered interactive dashboard featuring live execution streaming and localized version control.

---

## ✨ Part 1: LangGraph Orchestration & Intelligent Extraction

### The Problem
Linear LLM chains (like standard LangChain pipelines) fail in dynamic coding environments. They cannot effectively loop back for retries, manage complex multi-turn state histories, or seamlessly handle the transition from "generating a file" to "editing a file." Furthermore, LLMs frequently hallucinate markdown formatting or inject conversational filler, corrupting the code extraction process.

### The Solution
Implemented a highly intelligent, stateful architecture:
1. **Persistent State Management:** The `CoderState` strictly monitors the conversation memory (`messages`) and the living document (`current_code`).
2. **Agentic Routing:** The LLM acts as the central router. If the workspace is empty, it writes full code. If populated, it is mathematically constrained via Pydantic to invoke a patching tool.
3. **Bulletproof Extraction:** Engineered a multi-pass regex and heuristic parser that ignores markdown hallucinations and conversational text (e.g., "Here is your code:"), guaranteeing that only executable Python syntax enters the workspace.

> 📄 **Included:** An **ADR** and **Research Report** detailing the architectural decisions behind cloud-offloaded compute and LangGraph orchestration constraints.

---

## 📊 Part 2: Deterministic Diff Engine & UI/UX Engineering

### The Problem (Code Mutation)
Asking an LLM to rewrite an entire file for a 2-line logic change is an anti-pattern. It causes severe token exhaustion, destroys real-time latency, and drastically increases the risk of regression bugs or "lazy generation" (outputting `# ... rest of the code ...`). Standard Git unified diffs also fail due to the LLM's inability to accurately calculate absolute line numbers.

### The Solution (SEARCH/REPLACE Protocol)
We bypassed line-number arithmetic entirely by implementing a Semantic String Matching engine:
* **The `CodePatch` Tool:** A strict JSON schema requiring exact `search_block` and `replace_block` strings.
* **Algorithmic Fallbacks:** The Python backend attempts an exact string match. If the LLM hallucinates minor indentation, it falls back to a normalized whitespace matching algorithm, ensuring high success rates for surgical edits.

### Symmetrical UI & Local Version Control
The Jupyter interface was fundamentally re-engineered using `ipywidgets` and `threading`:
* **IDE Rendering:** Migrated from raw Markdown to `Pygments` (Monokai theme) for professional syntax highlighting.
* **Live Observability:** A dedicated terminal streams background LangGraph node executions and exact LLM token metrics dynamically.
* **Version History:** Implemented a persistent Tab system that logs the exact voice prompt alongside the resulting codebase state, allowing developers to visually audit the chronological evolution of their software.

---

## 🚀 How to Run

**1. Clone the repository**
```bash
git clone [https://github.com/Turing-Session07-Project.git](https://github.com/Turing-Session07-Project.git)
cd Agentic-Voice-Coder
```

**2. Install dependencies**
```bash
pip install -qU langgraph langchain-openai pydantic SpeechRecognition pyaudio ipywidgets langchain-community pygments
```

**3. Execution Environment**
This project requires a Jupyter environment (Jupyter Notebook or Jupyter Lab or Vs Code).

**4. Configure & Run**
Execute the notebook cells sequentially.
*Note: Ensure your microphone permissions are granted to your terminal or browser running the Jupyter kernel.*

---

## 🛠️ Tech Stack & Libraries

* **Core:** `Python`, `langgraph`, `langchain-openai`, `pydantic`
* **Voice & Modality:** `pyaudio`, `SpeechRecognition`, `wave`
* **State & Concurrency:** `threading`, `typing.TypedDict`
* **UI & Rendering:** `ipywidgets`, `IPython.display`, `pygments (Lexers/Formatters)`, `html`
* **Parsing Engine:** `re`
  
