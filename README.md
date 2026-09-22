# HealthBot AI 🩺🤖

An AI-powered health education assistant built with **Google Gemini, LangChain, LangGraph, and Tavily Search**.

HealthBot searches trusted health sources, generates a patient-friendly summary, creates a comprehension quiz, evaluates the user's answer, and allows the user to continue with another topic.

> **Disclaimer:** This project is for educational purposes only and is not a substitute for professional medical advice.

## 🚀 Features

* 🔎 Health information retrieval using **Tavily Search**
* 🤖 Patient-friendly summaries using **Google Gemini**
* 📝 Automatic quiz generation
* ✅ Answer evaluation and grading
* 🔄 Multiple health topics in one session
* 🧠 State-based workflow using **LangGraph**
* ♻️ State reset between topics

## 🛠️ Tech Stack

* **Python 3.11**
* **Google Gemini**
* **LangChain**
* **LangGraph**
* **Tavily Search**
* **Jupyter Notebook**
* **uv**

## 🔄 Workflow

```text
Health Topic
     ↓
Tavily Search
     ↓
Gemini Summary
     ↓
Quiz Generation
     ↓
User Answer
     ↓
Answer Grading
     ↓
Continue / Exit
```

The workflow is managed using **LangGraph** with a shared `HealthBotState` between nodes.

## 📁 Project Structure

```text
Healthbot AI/
├── HealthBot_AI.ipynb
├── src/
│   └── healthbot_ai/
├── pyproject.toml
├── requirements.txt
├── uv.lock
├── .python-version
├── .gitignore
└── README.md
```

## ⚙️ Setup

### 1. Clone the repository

```bash
git clone https://github.com/Moni-5/HealthBot-AI.git
cd HealthBot-AI
```

### 2. Create and activate the environment

```bash
pip install uv
uv venv --python 3.11
```

**Windows PowerShell:**

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
uv sync
```

Or:

```bash
pip install -r requirements.txt
```

### 4. Configure API Keys

Create `config.env` in the project root:

```env
GOOGLE_API_KEY="your_google_gemini_api_key"
TAVILY_API_KEY="your_tavily_api_key"
```

**Do not commit `config.env` to GitHub.**

### 5. Run

Open:

```text
HealthBot_AI.ipynb
```

in VS Code, select the `.venv` Python interpreter, and run the notebook cells from top to bottom.

## 🔐 Security

The following are excluded from Git:

```text
config.env
.venv/
__pycache__/
.ipynb_checkpoints/
```

Never expose API keys in the repository.

## ⚠️ Disclaimer

HealthBot AI provides information for **educational purposes only**. It should not be used for medical diagnosis or treatment decisions.
