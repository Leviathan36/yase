# Commands

## Installation

```bash
git clone <repository_url>
# Optionally, remove the git history
cd <repository_directory>
rm -rf .git
git init
# Install dependencies
poetry install
```

(Note: Changed the code block language to `bash` for these shell commands, and `python` to `bash` or `yaml` where appropriate in subsequent blocks if they represent shell commands or YAML content, respectively. If a block contains Python-specific syntax being commented on, `python` is kept.)

## Documentation

```bash
# Generates documentation for the entire app and places it in the "docs" folder.
# You can then navigate it by opening docs/index.html.
pdoc -o docs app
```

## Tests

```bash
bandit -r app/
```

```bash
bandit -r app/ -f json -o bandit-report.json
```

```bash
wapiti -u http://127.0.0.1:8000/ \
-x http://127.0.0.1:8000/logout \
-H "Authorization: Bearer <YOUR_JWT_TOKEN>" \
-f json \
-o report_wapiti.json
```

(Note: Changed the output filename in the Wapiti command to `report_wapiti.json` for consistency if it was named differently in another document, or to differentiate if needed. `<TOKEN_JWT>` should be replaced with an actual token.)

## Run app

```bash
# See: https://fastapi.tiangolo.com/deployment/manually/#use-the-fastapi-run-command
fastapi run main.py
```

(Note: The original `toml` for the code block language seemed incorrect for a run command; changed to `bash` as it's a shell command. If `main.py` is intended to be the content of a TOML file, the language hint should reflect that, but typically `fastapi run` is a CLI command.)
