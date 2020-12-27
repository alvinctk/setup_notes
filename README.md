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

Install `pyenv` via Homebrew by running:

```bash
brew install pyenv
```

When finished, Open your .bash_profile in the home directory (you can use code ~/.bash_profile), an add the following line to `~/.bash_profile`:



[Using pyenv to install and manage multiple python version](https://anil.io/blog/python/pyenv/using-pyenv-to-install-multiple-python-versions-tox/)

[tox automation project](https://pypi.org/project/tox/)

[tox documentation](https://tox.readthedocs.io/en/latest/)


### pip install fails for every package

Error: 

`pip install <package name>` is failing for every package for me. This is what I get:

```
Could not find a version that satisfies the requirement <package-name>
```
Fix: 


Upgrade pip as follows:
```bash
curl https://bootstrap.pypa.io/get-pip.py | python
```

Note: You may need to use sudo python above if not in a virtual environment.


Lastly, to avoid other install errors, make sure you also upgrade setuptools after doing the above:

```bash
pip install --upgrade setuptools
```

Further reads from [source](https://stackoverflow.com/questions/49748063/pip-install-fails-for-every-package-could-not-find-a-version-that-satisfies/49748494#49748494)


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
