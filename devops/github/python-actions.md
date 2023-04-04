# Python Github Action with Pipenv & Ruff

This is a Github action that installs dependencies via Pipenv and lints with
Ruff

```yml
name: Generic Python Action
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install pipenv
        run: |
          python -m pip install --upgrade pipenv wheel
      - id: cache-pipenv
        uses: actions/cache@v1
        with:
          path: ~/.local/share/virtualenvs
          key: ${{ runner.os }}-pipenv-${{ hashFiles('**/Pipfile.lock') }}

      - name: Install dependencies
        if: steps.cache-pipenv.outputs.cache-hit != 'true'
        run: |
          pipenv install --deploy --dev

      - name: Lint with Ruff
        run: |
          pip install ruff
          ruff --format=github --target-version=py37 .
        continue-on-error: true
```
