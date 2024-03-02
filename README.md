# Poetry 101
These steps are following the official documentation for python-poetry.
https://python-poetry.org/docs/

## Installation via ASDF
```
asdf install
export PATH="/Users/barbara/.asdf/installs/poetry/1.8.1/bin:$PATH"
```

## Set up Virtual Environment
```
python3 -m venv venv
. ./venv/bin/activate
```

## Project Set up
```
poetry new poetry-demo
```