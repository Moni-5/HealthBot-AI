# HealthBot Capstone — Local Setup Guide (VS Code)

This folder contains everything you need:

```
healthbot_project/
├── HealthBot.ipynb        <- the LangGraph workflow (main deliverable)
├── requirements.txt       <- pinned dependencies
├── config.env.example     <- rename to config.env and add your real keys
└── README.md              <- this file
```

## 1. Get your API keys

- **OpenAI**: https://platform.openai.com/api-keys
- **Tavily**: sign up free at https://app.tavily.com/home — first 1000 searches are free

## 2. Set up the project in VS Code (local machine)

Open a terminal in VS Code, `cd` into this folder, then:

```bash
# 1. Install uv (a fast Python package/venv manager)
pip install uv

# 2. Initialize the project (creates pyproject.toml etc. — safe to run even with existing files)
uv init --no-workspace

# 3. Create a virtual environment pinned to Python 3.11.13
uv venv --python 3.11.13

# 4. Check the venv's Python version
python --version
```

**Activate the virtual environment**

- Windows (PowerShell):
  ```powershell
  .\.venv\Scripts\Activate
  ```
- macOS/Linux:
  ```bash
  source .venv/bin/activate
  ```

**Install dependencies**

```bash
uv add -r requirements.txt
```

**Confirm what's installed**

```bash
pip list
```

## 3. Configure your API keys

Copy the template and fill in your real keys:

```bash
cp config.env.example config.env      # macOS/Linux
copy config.env.example config.env    # Windows
```

Edit `config.env` so it looks like:

```
OPENAI_API_KEY="sk-...your real key..."
TAVILY_API_KEY="tvly-...your real key..."
```

`config.env` must live in the **same folder** as `HealthBot.ipynb`.

## 4. Open and run the notebook in VS Code

1. Install the **Jupyter** and **Python** extensions in VS Code if you haven't already.
2. Open `HealthBot.ipynb`.
3. When prompted, select the kernel from `.venv` (the interpreter you just created).
4. Run cells top to bottom with Shift+Enter.
5. When you reach the "Run the HealthBot" cell, VS Code will show input boxes at
   the top of the editor for each `input()` prompt (topic, ready-for-quiz,
   quiz answer, continue/exit) — this is the same behavior as Jupyter's
   `input()` modal described in the assignment.

## 5. Workflow implemented (matches the assignment order exactly)

1. `get_topic` → asks the patient for a health topic
2. `search_tavily` → searches Tavily (biased toward Mayo Clinic, NIH, CDC, MedlinePlus)
3. `summarize` → LLM writes a 3–4 paragraph patient-friendly summary, using **only** the search results
4. `present_summary` → prints the summary, waits for the patient to confirm they're ready
5. `generate_quiz` → LLM writes one comprehension question from the summary **only**
6. `present_quiz` → shows the question, collects the patient's answer
7. `grade_answer` → LLM grades the answer (letter grade) using only the summary, with a citation-backed explanation
8. `present_grade` → shows the grade and explanation
9. `ask_continue` → asks if the patient wants another topic or wants to exit
10. Conditional edge: **reset state** and loop back to step 1, or **end** the graph

State is a single `TypedDict` (`HealthBotState`) that every node reads from and
writes to, so later nodes (especially quiz generation and grading) have access
to everything gathered earlier in the session. On loop-back, `reset_state_node`
wipes the state clean so no data from a prior topic leaks into the next session.

## 6. Moving this to your VM

Once it runs cleanly here:

1. Copy the **entire `healthbot_project` folder** to the VM (zip it, use `scp`,
   a shared drive, or your VM provider's file upload — whichever you normally use).
2. **Do not copy your real `config.env`** if the VM environment already provides
   keys a different way — check your assessment platform's instructions first.
   If you do need to bring your own keys, copy `config.env` too, but keep it
   out of any git repo (see `.gitignore` note below).
3. On the VM, repeat steps 2–4 above (create venv, install deps, confirm keys)
   to keep the environment clean and reproducible rather than copying your
   local `.venv` folder over (virtual environments generally shouldn't be
   copied between machines/OSes).

**Tip:** if you use git at any point, add this to a `.gitignore` so you never
commit real API keys:

```
config.env
.venv/
__pycache__/
.ipynb_checkpoints/
```

## 7. Rubric self-check before submitting

- [ ] API keys load without error, and both OpenAI and Tavily calls succeed (see the sanity-check cell)
- [ ] Summary is 3–4 paragraphs and uses only the Tavily results
- [ ] Quiz question is answerable from the summary alone
- [ ] Grading uses only the summary and includes a citation/justification
- [ ] State (`HealthBotState`) is referenced and updated by every node
- [ ] Full loop works end-to-end: topic → summary → quiz → grade → continue/exit
- [ ] Choosing "yes" to continue resets state and doesn't leak the previous topic's data
