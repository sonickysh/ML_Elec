# Python, VS Code, `venv`, and Jupyter Notebook Workflow

A practical guide for working on Python and machine-learning projects on
Windows using VS Code, a project-specific virtual environment, and
Jupyter notebooks.

------------------------------------------------------------------------

## 1. The basic idea

Keep each project self-contained:

-   **Python** runs your code.
-   **VS Code** is where you edit and manage the project.
-   **`.venv`** holds the Python interpreter environment and libraries
    for this project.
-   **Jupyter (`.ipynb`)** lets you work interactively in cells.
-   **`requirements.txt`** records the project's Python dependencies.
-   **Git** tracks your source files, not your virtual environment.

A project's `.venv` is usually specific to the computer, operating
system, and Python installation where it was created. Do not rely on a
`.venv` copied from another PC or included in a ZIP file. Recreate it
when moving the project to another computer.

## 2. Recommended project structure

For example:

``` text
ML_Elec/
├── .venv/                 # Local environment; do not commit to Git
├── notebooks/
│   ├── 1_linear_regression.ipynb
│   └── 2_model_evaluation.ipynb
├── data/
│   ├── raw/               # Original data; check privacy and Git rules
│   └── processed/
├── src/                   # Reusable Python code
├── outputs/               # Charts, reports, exported results
├── requirements.txt       # Project dependencies
├── .gitignore
└── README.md
```

You do not need every folder for every project. Start small and add
folders when they are useful.

**Important:** Open the project root (`ML_Elec`) in VS Code, not just an
individual notebook or the `notebooks` folder. This makes interpreter
selection, relative paths, and project settings easier to manage.

## 3. First-time setup on a PC

### Step 1 --- Install Python and VS Code

Install Python from
[python.org](https://www.python.org/downloads/windows/) and VS Code from
[code.visualstudio.com](https://code.visualstudio.com/).

During Python installation, enable **Add Python to PATH** if that option
is shown.

In VS Code, install these extensions:

-   **Python** --- Microsoft
-   **Jupyter** --- Microsoft

Flake8 or Ruff are optional linting tools. You do not need them just to
run Python or notebooks.

### Step 2 --- Open the project

1.  Extract the project ZIP if needed.
2.  Open VS Code.
3.  Choose **File → Open Folder**.
4.  Select the project's root folder.

If the ZIP includes a `.venv` folder created on another PC, you can
rename or remove that environment and create a new one. Keep the
project's source files and `requirements.txt`.

### Step 3 --- Check Python

Open the VS Code terminal using **Terminal → New Terminal**.

In PowerShell:

``` powershell
py --version
python --version
```

If `py` is available, it can be used to select an installed Python
version. Check the project's README or configuration for a required
Python version.

### Step 4 --- Create a virtual environment

Run this from the project root:

``` powershell
py -m venv .venv
```

If you do not have the Windows Python launcher (`py`), use:

``` powershell
python -m venv .venv
```

### Step 5 --- Activate it

PowerShell:

``` powershell
.\.venv\Scripts\Activate.ps1
```

Command Prompt:

``` cmd
.venv\Scripts\activate.bat
```

When activated, the terminal prompt normally starts with `(.venv)`.

If PowerShell blocks activation, you can use Command Prompt instead. You
can also run the environment's Python directly without activating it:

``` powershell
.\.venv\Scripts\python.exe --version
```

### Step 6 --- Upgrade pip and install project dependencies

If the project already has `requirements.txt`:

``` powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If there is no requirements file yet, install the packages the project
needs, as described in Section 6.

### Step 7 --- Install and register the Jupyter kernel

For notebook work, install Jupyter and IPython Kernel inside the active
`.venv`:

``` powershell
python -m pip install jupyter ipykernel
```

Register this environment as a notebook kernel:

``` powershell
python -m ipykernel install --user --name ml-elec --display-name "Python (ML_Elec)"
```

The kernel name (`ml-elec`) should be unique for each project if you
register multiple project environments.

### Step 8 --- Select the interpreter in VS Code

1.  Press `Ctrl + Shift + P`.
2.  Run **Python: Select Interpreter**.
3.  Select the interpreter inside the project:

``` text
.venv\Scripts\python.exe
```

This controls the Python interpreter VS Code uses for ordinary Python
files and many Python features.

### Step 9 --- Select the notebook kernel

Open an `.ipynb` file.

1.  Click **Select Kernel** in the notebook's upper-right corner.
2.  Choose **Python Environments** or the available kernel list.
3.  Select the project's `.venv` interpreter or the registered
    `Python (ML_Elec)` kernel.

The notebook kernel is what executes notebook cells. Do not assume it is
correct just because the VS Code terminal shows `(.venv)`.

Verify the kernel by running this cell:

``` python
import sys

print(sys.executable)
```

The output should point to your project's `.venv`, for example:

``` text
C:\Projects\ML_Elec\.venv\Scripts\python.exe
```

## 4. Your normal workflow when starting work

Use this routine each time you open the project:

1.  Open the project root folder in VS Code.
2.  Open a new terminal.
3.  Activate `.venv`.
4.  Check that the correct interpreter is selected.
5.  Open the notebook and check its kernel.
6.  Continue working.

PowerShell commands:

``` powershell
cd "C:\path\to\ML_Elec"
.\.venv\Scripts\Activate.ps1
python --version
python -m pip --version
```

If VS Code automatically activates the environment, you may see
`(.venv)` already. Check the prompt and interpreter rather than
activating repeatedly.

To leave the environment:

``` powershell
deactivate
```

Closing the terminal also ends that terminal's activated session.

## 5. How to use Jupyter notebooks (`.ipynb`)

A notebook is made of cells. A cell can contain Python code or Markdown
text.

### Run a cell

-   `Shift + Enter` --- run the current cell and move to the next one.
-   `Ctrl + Enter` --- run the current cell and stay on it.
-   Use the play button beside a cell to run it.

### Useful notebook actions

-   **Add cell:** use the `+ Code` or `+ Markdown` controls.
-   **Change cell type:** select Code or Markdown from the cell-type
    menu.
-   **Run all:** use the notebook's Run All command.
-   **Restart kernel:** restart the notebook's Python process and clear
    its in-memory variables.
-   **Clear outputs:** remove saved cell outputs when appropriate.

### Code cells and Markdown cells

Use code cells for Python:

``` python
import pandas as pd

df = pd.read_csv("../data/raw/example.csv")
df.head()
```

Use Markdown cells for explanations, headings, notes, and equations:

``` markdown
# Linear Regression

## Objective
Predict the target variable using the selected features.

### Observations
- Check missing values.
- Inspect feature distributions.
- Evaluate model performance.
```

### A useful notebook order for a data-analysis project

A clear notebook often follows this structure:

1.  **Title and objective** --- what question are you investigating?
2.  **Imports and configuration** --- import libraries and define paths
    or settings.
3.  **Load data** --- read the dataset.
4.  **Initial inspection** --- inspect shape, columns, types, and sample
    rows.
5.  **Data cleaning** --- handle missing values, duplicates, and invalid
    values.
6.  **Exploratory data analysis (EDA)** --- distributions,
    relationships, and visualizations.
7.  **Feature preparation** --- encode, scale, or transform features
    when needed.
8.  **Train/test split** --- separate data appropriately.
9.  **Model training** --- fit the model.
10. **Evaluation** --- use suitable metrics and diagnostics.
11. **Conclusion** --- summarize findings, limitations, and next steps.

You do not need all these sections in every notebook. Use the sections
that fit the task.

### Important: notebook execution order

Jupyter keeps variables in memory. A cell can use a variable created by
an earlier-executed cell, even if that cell appears above it.

This can cause confusing results if cells are run out of order.

Before considering a notebook complete:

1.  Save the notebook.
2.  Restart the kernel.
3.  Run all cells from top to bottom.
4.  Fix any errors.
5.  Save again.

If a notebook only works because of variables left over from earlier
experiments, it is not yet reliably reproducible.

### File paths in notebooks

Paths are often a source of errors. Prefer paths relative to the project
root or define a project path explicitly.

For example, if the notebook is in `notebooks/` and the CSV is in
`data/raw/`:

``` python
from pathlib import Path
import pandas as pd

PROJECT_ROOT = Path.cwd().parent
DATA_PATH = PROJECT_ROOT / "data" / "raw" / "example.csv"

df = pd.read_csv(DATA_PATH)
```

This example assumes the notebook's current working directory is the
`notebooks` folder. Check it with:

``` python
from pathlib import Path

print(Path.cwd())
```

If the working directory differs, adjust the project-root calculation
accordingly. Avoid hardcoding another person's absolute path, such as
`C:\Users\Someone\Desktop\...`.

## 6. What to do when you install a new library

Always install libraries into the project's active `.venv`, not into an
unrelated global Python installation.

### Step 1 --- Activate the project environment

``` powershell
.\.venv\Scripts\Activate.ps1
```

### Step 2 --- Install the library

For example, to install CatBoost:

``` powershell
python -m pip install catboost
```

Use `python -m pip` rather than just `pip`. This helps ensure pip
belongs to the Python interpreter you intend to use.

Other examples:

``` powershell
python -m pip install pandas
python -m pip install scikit-learn
python -m pip install matplotlib
python -m pip install shap
```

The name used with pip can differ from the name used in Python imports.
For example:

  Install command               Python import
  ----------------------------- ------------------------------------------
  `pip install scikit-learn`    `import sklearn`
  `pip install opencv-python`   `import cv2`
  `pip install Pillow`          `from PIL import Image`
  `pip install catboost`        `from catboost import CatBoostRegressor`

### Step 3 --- Verify the library

Check that it is installed:

``` powershell
python -m pip show catboost
```

Or test it in Python:

``` python
import catboost

print(catboost.__version__)
```

### Step 4 --- Update `requirements.txt`

After installing a new dependency, update the project's dependency file:

``` powershell
python -m pip freeze > requirements.txt
```

This records all packages installed in the environment, including
transitive dependencies.

**Important:** `pip freeze` records the entire environment. If you
installed temporary or unrelated packages, they will also appear. For a
small project, this may be acceptable. For a carefully maintained
project, keep a deliberate list of direct dependencies and pin versions
according to your team's workflow.

Do not manually edit a dependency file without checking that the
environment can still be recreated.

### Step 5 --- Verify dependencies

``` powershell
python -m pip check
```

Then test the import and rerun the relevant notebook cells or tests.

### What if a library is already installed?

Check first:

``` powershell
python -m pip show pandas
```

If it is already installed in the active environment, you generally do
not need to install it again.

## 7. How to use `requirements.txt`

`requirements.txt` is the dependency list used to recreate an
environment.

### Install all listed dependencies

``` powershell
python -m pip install -r requirements.txt
```

### Save the current environment

``` powershell
python -m pip freeze > requirements.txt
```

### Check installed packages

``` powershell
python -m pip list
```

### Check a specific package

``` powershell
python -m pip show numpy
```

### Recreate an environment from scratch

If the environment becomes corrupted or you need to rebuild it, close
notebook kernels and terminals using it, then:

1.  Remove the old `.venv` folder.
2.  Create a fresh environment.
3.  Activate it.
4.  Install `requirements.txt`.
5.  Register or select the Jupyter kernel again.

Commands:

``` powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install jupyter ipykernel
```

If the requirements file already includes Jupyter and IPython Kernel,
the last installation command is unnecessary. Select the recreated
`.venv` as the notebook kernel.

## 8. Git and `.gitignore`

Do not commit the `.venv` directory. It is large, machine-specific, and
reproducible from the dependency file.

A useful `.gitignore` for a Python and Jupyter project:

``` gitignore
# Virtual environments
.venv/
venv/
env/

# Python cache
__pycache__/
*.py[cod]

# Jupyter
.ipynb_checkpoints/

# Environment variables and secrets
.env
.env.*

# Editor settings (optional)
.vscode/

# Generated files
outputs/
```

Only ignore `outputs/` if generated outputs should not be
version-controlled. Likewise, do not ignore data blindly: follow the
project's privacy, licensing, and collaboration rules.

Never commit passwords, API keys, access tokens, or private datasets.

## 9. Troubleshooting

### `ModuleNotFoundError`

Example:

``` text
ModuleNotFoundError: No module named 'catboost'
```

Likely cause: the package is missing from the environment used by the
notebook.

Check the notebook interpreter:

``` python
import sys
print(sys.executable)
```

Then install using that environment. In a notebook, you can also run:

``` python
%pip install catboost
```

The `%pip` magic is useful because it installs into the environment
associated with the current notebook kernel. Restart the kernel if the
newly installed package is not recognized.

### Notebook kernel not found

1.  Confirm `.venv` exists.
2.  Activate it in the terminal.
3.  Install `ipykernel` if missing.
4.  Re-select the notebook kernel.

``` powershell
python -m pip install ipykernel
```

### Terminal uses the wrong Python

Run:

``` powershell
python -c "import sys; print(sys.executable)"
```

Then use **Python: Select Interpreter** in VS Code to select
`.venv\Scripts\python.exe`.

### `pip` installs a package but the notebook cannot import it

The terminal and notebook may be using different Python environments.

Compare:

Terminal:

``` powershell
python -c "import sys; print(sys.executable)"
```

Notebook:

``` python
import sys
print(sys.executable)
```

The paths should point to the same project environment.

### PowerShell refuses to activate `.venv`

Use Command Prompt:

``` cmd
.venv\Scripts\activate.bat
```

Or run the environment's Python executable directly:

``` powershell
.\.venv\Scripts\python.exe -m pip install pandas
```

### `pip check` reports dependency conflicts

Read the package names and version requirements in the output. Do not
randomly upgrade every package. Check the project's Python version and
dependency constraints, then resolve the conflicting versions
deliberately.

### Flake8 reports warnings in `.ipynb` files

Flake8 is a linter, not the notebook execution engine. Its diagnostics
do not automatically mean the notebook cannot run.

Examples:

-   `F401`: imported name is unused.
-   `E231`: missing whitespace after a comma.
-   `E225`: missing whitespace around an operator.
-   `E402`: import is not at the top of a module; this can be awkward in
    notebooks because cells are executed independently.

You can keep linting for `.py` files and configure the Flake8 extension
to exclude notebooks if that suits your workflow. Check the extension's
current settings and output if diagnostics persist.

## 10. A simple daily checklist

When beginning work:

-   [ ] Open the project root in VS Code.
-   [ ] Activate the correct `.venv`.
-   [ ] Check the selected Python interpreter.
-   [ ] Check the notebook kernel.
-   [ ] Confirm required data files are available.

When adding a library:

-   [ ] Install it into the active project environment.
-   [ ] Verify the import or package version.
-   [ ] Update `requirements.txt`.
-   [ ] Run `python -m pip check`.
-   [ ] Test the affected code.

Before finishing work:

-   [ ] Save notebooks and Python files.
-   [ ] Restart and run important notebooks from top to bottom.
-   [ ] Save dependency changes.
-   [ ] Check Git changes.
-   [ ] Ensure `.venv`, secrets, and private data are not accidentally
    committed.

------------------------------------------------------------------------

## 11. Quick command reference

  Task                     Command
  ------------------------ -------------------------------------------------
  Create environment       `py -m venv .venv`
  Activate (PowerShell)    `.\.venv\Scripts\Activate.ps1`
  Activate (CMD)           `.venv\Scripts\activate.bat`
  Deactivate               `deactivate`
  Check Python             `python --version`
  Check Python path        `python -c "import sys; print(sys.executable)"`
  Upgrade pip              `python -m pip install --upgrade pip`
  Install a package        `python -m pip install package-name`
  Install requirements     `python -m pip install -r requirements.txt`
  Save dependencies        `python -m pip freeze > requirements.txt`
  List packages            `python -m pip list`
  Inspect a package        `python -m pip show package-name`
  Check conflicts          `python -m pip check`
  Install Jupyter kernel   `python -m pip install jupyter ipykernel`
  Run a Python script      `python main.py`

## 12. The most important rules

1.  **One project, one virtual environment.** Avoid sharing one
    environment across unrelated projects.
2.  **Install packages into the active project environment.** Use
    `python -m pip install ...`.
3.  **Keep the VS Code interpreter and notebook kernel aligned.** Verify
    with `sys.executable`.
4.  **Do not share or commit `.venv`.** Share source files and
    dependency instructions instead.
5.  **Update `requirements.txt` when dependencies change.**
6.  **Restart and run notebooks from top to bottom before treating them
    as reproducible.**
7.  **Keep notebooks organized.** Use Markdown to explain the analysis
    and code cells for execution.
8.  **Check paths and data availability.** Avoid machine-specific
    absolute paths.

Following this workflow makes your projects easier to reproduce, move
between computers, troubleshoot, and share with teammates.
