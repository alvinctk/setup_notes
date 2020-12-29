# setup_notes
The setup notes, served as a personal reference for myself, describes how I setup my developer environment on macOS big
Sur. 

This document desribes how I sestup my developer environment on macOS Big Sur. 

Contributing: If you find any mistakes in the steps describe below, or if any
of the commands are outdated, do let me know. 

## CLI - Command Line Developer Tools for Xcode

Important dependency before Homebrew can work is the **Command Line Developer
Tools** for Xcode. These includes compilers that will allow you to build things
from source. Install CLI directly on the terminal:

```bash
xcode-select install
```

## Homebrew 

Once CLI is installed, we can install Homebrew. [Homebrew](https://brew.sh) is
the missing package manager for macOS (or Linux). 


Paste the following in a macOS Terminal or Linux shell prompt. 
```bash
/bin/bash -c "$(curl -fsSL
https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```





## git setup

### Connecting to Github with ssh on MacOS 

0. [Configure ssh setup on macOS terminal](http://kbroman.org/github_tutorial/pages/first_time.html)

1. [Checking for existing SSH keys](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/checking-for-existing-ssh-keys)

2. [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

3. [Testing your ssh connection to github account](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/testing-your-ssh-connection)

4. [Using SSH authentication on terminal](https://gist.github.com/ddeveloperr/1859fd395e7cb5832c59)

## python setup 
This python setup guide use in part of the following:

- [A gist about python_environment.md](https://gist.github.com/wronk/a902185f5f8ed018263d828e1027009b#using-the-workflow)
- [Homebrew Documentation on Python](https://docs.brew.sh/Homebrew-and-Python)
- [Using pyenv to install and manage multiple python
  version](https://anil.io/blog/python/pyenv/using-pyenv-to-install-multiple-python-versions-tox/)
- [tox automation project](https://pypi.org/project/tox/)
- [tox documentation](https://tox.readthedocs.io/en/latest/)


Follow the order of steps in order to setup Python.


### Python build dependencies

1. Install [Python build dependencies](https://github.com/pyenv/pyenv/wiki#suggested-build-environment) before attempting to install a new Python
   version.

**Notes**: Highly recommend to install Python build dependencies before any
installation with Python and its related modules. 

```bash
# optional, but recommended:
brew install openssl readline sqlite3 xz zlib
```


### `pyenv`
[`pyenv`](https://github.com/pyenv/pyenv) stands for Python environment. `pyenv` lets you easily switch between multiple versions of Python. It's simple, unobtrusive, and follows the UNIX tradition of single-purpose tools that do one thing well.

According to the [pyenv guide](https://github.com/pyenv/pyenv#homebrew-on-macos): 

2. Install `pyenv` via Homebrew by running:

```bash
brew install pyenv
````
Also install [`pyenv-virtualenv`](https://github.com/pyenv/pyenv-virtualenv)
like `brew install pyenv-virtualenv`, which we'll need later. 

**Technical details**: Every time you execute a Python script or use pip, pyenv works in the background to intercept that command and send it to the Python environment that is currently activated. It does this using shims on the PATH environment variable, which allow Python-related commands to be dynamically rerouted.


3. Define environment variable PYENV_ROOT to point to the path where pyenv repo is cloned and add $PYENV_ROOT/bin to your $PATH for access to the pyenv command-line utility.

For bash:
```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
```
For Ubuntu Desktop:
```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
```
For Zsh:
```zsh
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
````

4. Add pyenv init to your shell to enable shims and autocompletion. Please make sure eval "$(pyenv init -)" is placed toward the end of the shell configuration file since it manipulates PATH during the initialization.

```bash
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
```
- **Zsh note**: Modify your `~/.zshrc` file instead of `~/.bash_profile`.
- **fish note**: Use `pyenv init - | source` instead of `eval (pyenv init -)`.
- **Ubuntu and Fedora note**: Modify your `~/.bashrc` file instead of `~/.bash_profile`

**General warning**: There are some systems where the `BASH_ENV` variable is configured to point to `.bashrc`. On such systems you should almost certainly put the above mentioned line `eval "$(pyenv init -)"` into `.bash_profile`, and not into `.bashrc`. Otherwise you may observe strange behaviour, such as pyenv getting into an infinite loop. See [#264](https://github.com/pyenv/pyenv/issues/264) for details.

5. Restart your shell so the path changes take effect. You can now begin using pyenv
```bash
exec "$SHELL"
```
6. Install Python version

```bash
pyenv install 3.9.1 #Install Python version 3.9.1
pyenv install 3.7.2 #Install Python version 3.7.2
pyenv versions # List all Python versions installed
````


[virtualenv installation](https://virtualenv.pypa.io/en/stable/installation.html)



### pip 
According to [pip manual](https://pip.pypa.io/en/stable/installing/#install-pip), pip is already installed if you are using Python 2 >=2.7.9 or PYthon 3 >= 3.4 downloaded from [python.org](python.org) or if you are working in a virtual environment created by virtualenv or venv. Just make sure to upgrade pip. 


Check if pip is already installed: 
```bash
python -m pip --version 
```

### pip install fails for every package
According to [stackoverflow post](https://stackoverflow.com/questions/49748063/pip-install-fails-for-every-package-could-not-find-a-version-that-satisfies/49748494#49748494)

Error: 

`pip install <package name>` is failing for every package for me. This is what I get:

```
Could not find a version that satisfies the requirement <package-name>
Not matching distribution found for <package-name>
```
Fix: 


Upgrade pip as follows:
```bash
curl https://bootstrap.pypa.io/get-pip.py | python
```

The detailed description can be found in the [link](https://bhch.github.io/posts/2017/04/fix-the-pip-error-couldnt-find-a-version-that-satisfies-the-requirement/) shared by Anupam in the comments.

Note: You may need to use sudo python above if not in a virtual environment.


Lastly, to avoid other install errors, make sure you also upgrade setuptools after doing the above:

```bash
pip install --upgrade setuptools
```



## PostgreSQL setup 

The process is a lot easier to install `postgresql`.

0. Update homebrew 
```bash
brew doctor
brew update
```
1. Install postgresql 
```bash
brew install postgresql
````

2. Install `brew services`
```bash
brew tap homebrew/services
```
3. Other commands

3a. Start postgresql:
```bash
brew services start postgresql
```

3b. Stop postgresql: 
```bash
brew services stop postgresql 
```

3c. Display running background services
```bash
brew services list
```

[Postgresql installation on macOS](https://www.robinwieruch.de/postgres-sql-macos-setup)

[Starting and Stopping Background Services with Homebrew](https://thoughtbot.com/blog/starting-and-stopping-background-services-with-homebrew)

[Installing Postgres via Brew](https://gist.github.com/sgnl/609557ebacd3378f3b72)
