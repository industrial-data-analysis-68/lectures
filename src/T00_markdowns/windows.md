# Preparation: Windows

## 1. Set Execution Policy in PowerShell 5

> PowerShell checks the execution policy before running a script. If the script doesn’t meet the requirements (like source or signature), PowerShell blocks its execution. We need to allow scripts to be run.

- Open `PowerShell` with administrative right
- If you use Win 32

  - `New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force`

- `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine`

## 2. Install `choco`

> Choco is the command-line tool for Chocolatey, a popular package manager for Windows that automates software installation, updating, and removal using simple commands.

- `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`

## 3. Install PowerShell 7

- Open Command Prompt (`cmd`) as an administrator.
- `choco install powershell-core`

## 4. Set PowerShell 7 as Default in Windows Terminal

- Open Windows Terminal (you can search for it in the Start menu).
  -Click the dropdown arrow next to the tabs, then select Settings.
- Under Startup (or General), look for Default profile.
- Choose `PowerShell 7`
  - It might be listed as `PowerShell` with a higher version number, like `pwsh`.
- Click `save`.

## 5. Install `uv`

- `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
- Close and reopen powershell

# 6. Install Python

- `uv python install 3.12`

# 7. Create virtual environment

- `uv venv --python 3.12`
- Activate the environment
  - `~/.venv/Scripts/activate.ps1`
- Install packages
  - `uv pip install jupyterlab ipykernel pandas scikit-learn matplotlib seaborn openpyxl ruff notebook xlsxwriter`

# 8. Install VSCode

- Reopen Terminal with administrative right
- `choco install vscode -y`

# 9. Setup VSCode for Python

- Install VSCode extensions
  - Python Extension Pack
  - Jupyter
  - Ruff
- Go to VSCode setting
  - `python.venvPath` -> `~`
  - `editor.formatOnSave` -> ✔️

# 10. Run python file

- Create `hello.py`

```python
import pandas as pd

print("Hello World")
print(pd.__version__)
```

- Set "Workspace Environment"
- Run file from the play button.
- (Optionally) select formatter

# 11. Run Jupyter notebook

- Create `hello.ipynb`
- Click `Select Kernel` at the top right.
- Click `+ Code` and type some code.
- Click play button.
- (Optionally) select formatter
