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
