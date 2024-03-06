# Poetry 101
These steps are following the official documentation for python-poetry.
https://python-poetry.org/docs/

## Prerequisites
1. Test PyPI account
2. Test PyPI token (set your Pypi token now: `export PYPI_TOKEN=pypi-xxxx`)
3. Python 3.8+ installed
4. Poetry installed

## Installation via ASDF
There are other ways to install poetry, see https://python-poetry.org/docs/.

For this demo, I am using [asdf](https://asdf-vm.com/). 
ASDF is a version manager for various tools. The safety of using asdf is debatable.

```
asdf install

# check if poetry is in path
poetry -v
```

## Project Set up
Replace <test-pypi-username> with your test-pypi username, so there are no 
package name conflicts when we upload.

```
export PROJECT_NAME=<test-pypi-username>-poetry-demo
poetry new $PROJECT_NAME
```
## Poetry Dependencies
We don't actually need pendulum, we'll just use it as an example for adding and
removing dependencies.
```
cd $PROJECT_NAME
poetry add pendulum
# take a look at the pyproject.toml
poetry remove pendulum
# take a look at the pyproject.toml
```
Add pytest
```
poetry add pytest
```
## Add function and tests

### File Structure
Your file structure should look like this.
```
.
├── README.md
├── <test-pypi-username>_poetry_demo
│   ├── __init__.py
│   └── example.py
├── poetry.lock
├── pyproject.toml
└── tests
    ├── __init__.py
    └── test_example.py
```
### Steps
1. Create a file `${PROJECT_NAME//-/_}/example.py`:
```
def add_one(number):
    return number + 1


if __name__ == "__main__":
    print("This is example.py")
```
2. Add a file called `tests/test_example.py` in the tests/ folder with the following.
Make sure to replace the <PROJECT_NAME>:
```
from <PROJECT_NAME> import example

def test_add_one():
    assert example.add_one(2) == 3
```

## Poetry Build
```
poetry install
poetry run python ${PROJECT_NAME//-/_}/example.py
poetry run pytest
poetry build
```
## Set up Test PypI Repo
```
#if you didn't already, set PYPI_TOKEN=pypi-xxxxxxx with your token
poetry config repositories.testpypi https://test.pypi.org/legacy/
poetry config pypi-token.testpypi $PYPI_TOKEN
poetry publish -r testpypi --build

# expect and error, update to a "valid email"
# if you get an error about version name, update the version in pyproject.toml

```

## Test using Poetry shell
```
poetry shell
poetry run python
# replace PROJECT_NAME with the project (use underscores instead of hyphens)
>>> from <PROJECT_NAME> import example
>>> example.add_one(2)
3
```

To exit out of poetry shell: `deactivate`

## Test it out in the Python console

### Set up Virtual Environment
Poetry uses its own virtual environment, but this will be used when we test our 
package at the end of this demo.
```
python3 -m venv venv
. ./venv/bin/activate
pip install pytest
pip install -i https://test.pypi.org/simple/ ${PROJECT_NAME}
```
### Test the Package in the Console
Navigate to some other folder, so you know you're not using the local package/module.
`python3`
```
# replace PROJECT_NAME with the project (use underscores instead of hyphens)
>>> from <PROJECT_NAME> import example
>>> example.add_one(2)
3
```
