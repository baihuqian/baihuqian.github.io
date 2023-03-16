---
layout: "post"
title: "Set Up Python Environment"
date: "2020-02-01 15:18"
tags: Python
---

Two categories of setup are required to develop or run Python projects:

1. A suitable Python version. While the latest is always the greatest, some older projects may not work with the latest Python version.
2. Dependencies of a project. Different projects may require different or even conflicting dependencies, so isolated "virtual environments" is the best.

I'm going to focus only on Python 3, given that Python 2 is on the deprecation path for a long time. Since Python 3.3, the `venv` package is included in Python distribution as the recommended way to create lightweight virtual environments. To create a new environment at directory `path/to/virtualenv_name`, simply run `python3 -m venv path/to/virtualenv_name`. The `bin/activate` script in this directory can be used to activate the virtual environment: `source path/to/virtualenv_name/bin/activate`. You will find your terminal prompt prefixed with the name of the virtual environment, `virtualenv_name`, in parentheses. To deactivate the virtual environment, run `deactivate`, which is added to your PATH when `activate` is run.

When the environment is active, any packages can be installed to it via `pip` as normal. By default, the newly created environment will not include any packages already installed on the machine. As `pip` itself will not necessarily be installed on the machine. It is recommended to first upgrade `pip` to the latest version, using `pip install --upgrade pip`.

To install dependencies in the virtual environment, run `pip install -r requirements.txt`.

In many cases, projects require specific Python version, or some does not work with newer versions of Python. [`pyenv`](https://github.com/pyenv/pyenv) is a commonly used tool to install and manage Python versions. If you're a Ruby developer, you may be familiar with `rbenv`. `pyenv` is forked from it. Run `curl https://pyenv.run | bash` to install `pyenv`, and you need to set up shell environment to for it.

`pyenv install -l` shows all available versions, and `pyenv install <version>` installs a specific version of Python. To select a Pyenv-installed Python as the version to use, run one of the following commands:

* `pyenv shell <version>` -- select just for current shell session
* `pyenv local <version>` -- automatically select whenever you are in the current directory (or its subdirectories)
* `pyenv global <version>` -- select globally for your user account

`pyenv` looks in four places to decide which version of Python to use, in priority order:

1. The `PYENV_VERSION` environment variable (if specified). You can use the `pyenv shell` command to set this environment variable in your current shell session.
2. The project-specific `.python-version` file in the current directory (if present). You can modify the current directory's `.python-version` file with the `pyenv local` command.
3. The first `.python-version` file found (if any) by searching each parent directory, until reaching the root of your filesystem.
4. The global version file. You can modify this file using the `pyenv global` command. If the global version file is not present, pyenv assumes you want to use the "system" Python. (In other words, whatever version would run if pyenv weren't in your `PATH`.)

Now, the workflow to configure the Python environment for a project is:

1. Go to the environment root directory.
2. Set the local Python version using `pyenv local` command.
3. Create a virtual environment using `python3`, which points to the locally-enabled python version.
4. Activate the virtual environment.
5. Check out projects, install dependencies, etc.
