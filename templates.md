

# Templates with cookiecutter

Documentation in [https://cookiecutter.readthedocs.io/en/1.7.2/](https://cookiecutter.readthedocs.io/en/1.7.2/)

## Template structure

We are going to design a python template with the structure shwon below. The advantage of using a template is to aviod doing the same process many times. 


Makefile                       <- File containing shell commands
requirements-dev.txt  <- List of python libraries for development
requirements.txt         <- List of python libraries for production

```
library-template
|-- {{cookiecutter.repo_name}}
|   |-- {{cookiecutter.repo_name}}
|   |   |-- data
|   |   |   `-- __init__.py
|   |   |-- models
|   |   |   `-- __init__.py
|   |   |-- utils
|   |   |   `-- __init__.py
|   |   `-- __init__.py
|   |-- Makefile
|   |-- README.rst
|   |-- requirements-dev.txt
|   `-- requirements.txt
|-- README.md
`-- cookiecutter.json
```  

The files that are necessary to create a template are:
- library-template/cookiecutter.json
- library-template/README.md


```bash
mkdir -p library-template/
```

**library-template/cookiecutter.json**
```json
{
    "project_name": "Full project name",
    "project_short_name": "Short project name",
    "repo_name": "{{ cookiecutter.project_short_name.lower().replace(' ', '_').replace('-', '_') }}",
    "description": "A short description of the project.",
    "version": "0.1.0"
}
```


## Package configuration

### Create structure

```bash
mkdir -p library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/data
mkdir -p library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/models
mkdir -p library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/utils
touch library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/data/__init__.py
touch library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/models/__init__.py
touch library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/utils/__init__.py
touch library-template/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/__init__.py

```


**library-template/{{cookiecutter.repo_name}}/REAMDE.md**

{{cookiecutter.project_name}}
==============================

{{cookiecutter.description}}

## Getting started



### Python environment

Create env and install libraries

```bash
conda update -n base conda
conda create --prefix  $HOME/.conda/envs/{{ cookiecutter.repo_name }}
conda activate $HOME/.conda/envs/{{ cookiecutter.repo_name }}
pip install -r requirements.txt
```

Install these python libraries only for development

```bash
make pydev
```


**library-template/{{cookiecutter.repo_name}}/Makefile**

```makefile
# Makefile

# This generates a symbolic link (shortcut) where all the data is shared.
datalink:
	mkdir data

# Create a symbolic link to model directory on labshare:
modellink:
	mkdir models

# This creates a conda environment named as {{ cookiecutter.repo_name }}

# Install python libraries for development
pydev:

	pip install -r requirements-dev.txt
	pip install ipykernel
	ipython kernel install --user --name {{ cookiecutter.repo_name }}

# Any other Linux libraries to be installed
install:
	echo "To be defined"
```


```python
"""Setup file for ingredients."""
import pathlib
import setuptools

current_dir = pathlib.Path(__file__).parent
readme = (current_dir / "README.md").read_text()

setuptools.setup(
	name={{cookiecutter.project_name}},
	version="0.0.1",
	description={{cookiecutter.description}},
	long_description=readme,
	long_description_content_type="text/markdown",
	url="https://github.com/eegkno/ingredients.git",
	author="Edgar Garcia Cano",
	author_email="eegkno@gmail.com",
	classifiers=["Programming Language :: Python"],
	packages=setuptools.find_packages(),
	python_requires=">=3.7",
)
```

### Readme for the template

**library-template/README.md**
```markdown
# Cookiecutter Template

## Requirements to use the cookiecutter template:
 - Python 2.7 or 3.5

Note: Run the commands at the terminal.

Install with pip

>pip install cookiecutter

or

Install with conda

>conda config --add channels conda-forge
>conda install cookiecutter

## To start a new project, run:

Clean template

>cookiecutter url/to/git/repo

Using one of the available branches

>cookiecutter -c branch url/to/git/repo
```
