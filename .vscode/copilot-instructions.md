# VS Code + AI Assistant Instructions for CSOL-208

This file provides context to AI coding assistants (GitHub Copilot, Continue, Antigravity) about project setup and coding conventions.

## Environment Setup

**Goal**: A Python environment (Conda or venv) with `requirements.txt` installed, active in VS Code.

### 1. Check Git
Run `git --version` in terminal.
- If missing on **Windows**: Install [Git for Windows](https://gitforwindows.org/).
- If missing on **Mac**: Run `xcode-select --install`.

### 2. Create Python Environment

**Option A: Conda (Preferred)**
If user has Miniconda/Anaconda:
1. `conda create -n csol208 python=3.11 -y`
2. `conda activate csol208`
3. `pip install -r requirements.txt`

**Option B: venv (No Conda)**
1. `python3 -m venv .venv`
2. **Windows**: `.venv\Scripts\activate` | **Mac/Linux**: `source .venv/bin/activate`
3. `pip install -r requirements.txt`

### 3. VS Code Activation
1. Open any `.py` file.
2. Click the interpreter version in the bottom-right status bar (or `Ctrl+Shift+P` -> "Python: Select Interpreter").
3. Select `csol208` (Conda) or `.venv` (Virtual Env).
4. Restart terminal to ensure it activates automatically.


## Coding Conventions

### Data Operations: Use Ibis

We use **ibis** for all data manipulation. Ibis provides a pandas-like API that compiles to DuckDB, enabling work with larger-than-RAM datasets.

```python
import ibis
ibis.options.interactive = True

# Load data
co2 = ibis.read_csv("data/owid-co2-data.csv")

# Filter and select (lazy evaluation)
world = (
    co2
    .filter(co2.country == "World")
    .filter(co2.year >= 1900)
    .select("year", "co2")
)

# Execute and get pandas DataFrame when needed
world.to_pandas()
```

**Do NOT use pandas directly for data manipulation.** Ibis syntax is similar but scales better.

### Visualization: Use Altair

We use **altair** for all visualization. It produces clean, declarative code and works well with Streamlit dashboards.

```python
import altair as alt

chart = (
    alt.Chart(df)
    .mark_line()
    .encode(
        x="year:Q",
        y="co2:Q",
        color="country:N"
    )
    .properties(width=600, height=400)
)
chart.save("output.html")
```

**Do NOT use matplotlib.** It's too verbose for teaching.

### Style Guidelines

- Use method chaining with parentheses for readability
- Put each method on its own line
- Use descriptive variable names
- Add comments explaining *why*, not *what*

---

## Project Structure

- `data/` — CSV, Parquet files (gitignored if large)
- `notebooks/` — Jupyter notebooks for exploration  
- `src/` — Python scripts for reusable code
- `outputs/` — Generated charts (gitignored, regenerable)

---

## Dependencies

Core packages for this module:
- `ibis-framework[duckdb]` — Data operations with DuckDB backend
- `altair` — Declarative visualization
- `pandas` — For final data exchange (used sparingly)
- `jupyter` — Interactive notebooks

For later sessions:
- `streamlit` — Interactive dashboards

---

## Course Context

This is Module 1 of Climate Solutions 208 (UC Berkeley). Sessions cover:
1. IDE setup and GitHub
2. First data analysis with OWID CO2 data
3. Data cleaning and validation
4. Interactive dashboards with Streamlit

Students are learning AI-assisted coding. Help them understand *why* code works, not just *what* it does.
