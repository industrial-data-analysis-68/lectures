# Python Installation - Mac

## 1. Install `brew`

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 2. Add `brew` to your PATH

Check the location of `brew`

- `eval "$(/opt/homebrew/bin/brew shellenv)"`
  - If no error, use `echo 'eval "$(/opt/homebrew/bin/brew)"' >> $HOME/.zprofile`

- `eval "$(/usr/local/bin/brew shellenv)"`
  - If no error, use `echo 'eval "$(/usr/local/bin/brew)"' >> $HOME/.zprofile`

Restart terminal and check `brew` command works.

## 3. Install `uv`

- `brew install uv`

## 4. Install Python

- `uv python install 3.12`
- If you see permission error,
  - `sudo uv python install 3.12`

If you see warning about PATH,

```
echo 'export PATH="$PATH:/your/new/path"' >> ~/.zprofile
```

## 5. Create a virtual environment

- `uv venv --python 3.12`
- Activate the environment
  - `source ~/.venv/bin/activate`
- Install packages
  - `uv pip install jupyterlab ipykernel pandas scikit-learn matplotlib seaborn openpyxl ruff notebook xlsxwriter`

## 6. VSCode

- Install `VSCode`
  - `brew install --cask visual-studio-code`
- Install VSCode extensions
  - Python
  - Python Environments
  - Python Indent
  - Jupyter
  - Ruff
- Go to VSCode setting
  - `python.venvPath` -> `~`
  - `editor.formatOnSave` -> ✔️

## 7. Run python file

- Create `hello.py`

```python
import pandas as pd

print("Hello World")
print(print(pd.__version__))
```

- Set "Workspace Environment"
- Run file from the play button.
- (Optionally) select formatter

## 8. Run Jupyter notebook

- Create `hello.ipynb`
- Click `Select Kernel` at the top right.
- Click `+ Code` and type some code.
- Click play button.
- (Optionally) select formatter
