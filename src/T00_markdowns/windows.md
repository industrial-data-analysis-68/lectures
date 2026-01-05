# Preparation: Windows

## 1. Set Execution Policy in PowerShell 5

> PowerShell checks the execution policy before running a script. If the script doesn’t meet the requirements (like source or signature), PowerShell blocks its execution. We need to allow scripts to be run.

- Open `PowerShell` with administrative right
- If you use Win 32

  - `New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force`

- `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine`

## 2. Install `uv`

- `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
- Close and reopen powershell

# 3. Install Python

- `uv python install 3.12`

# 4. Create virtual environment

- `uv venv --python 3.12`
- Activate the environment
  - `~/.venv/Scripts/activate.ps1`
- Install packages
  - `uv pip install jupyterlab ipykernel pandas scikit-learn matplotlib seaborn openpyxl ruff notebook xlsxwriter`

# 5. Install VSCode

- Download and install from https://code.visualstudio.com/

# 6. Setup VSCode for Python

- Install VSCode extensions

  - Python
  - Python Environments
  - Python Indent
  - Jupyter
  - Ruff

- Go to VSCode setting
  - `python.venvPath` -> `~`
  - `editor.formatOnSave` -> ✔️

# 7. Run python file

- Create `hello.py`

```python
import pandas as pd

print("Hello World")
print(pd.__version__)
```

- Set "Workspace Environment"
- Run file from the play button.
- (Optionally) select formatter

# 8. Run Jupyter notebook

- Create `hello.ipynb`
- Click `Select Kernel` at the top right.
- Click `+ Code` and type some code.
- Click play button.
- (Optionally) select formatter
