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

## [Upgrade bash in macOS tutorial](https://www.shell-tips.com/mac/upgrade-bash/):
Due to licensing issues, macOS shows a bash version 3.2.

```bash
$> bash --version
GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin20)
Copyright (C) 2007 Free Software Foundation, Inc.
````

> Newer versions of macOS (Starting from Catalina) now users `zsh` as default instead of `bash`. 

#### Install the required `homebrew` 
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### Install bash updated version via homebrew
```bash
brew install bash
```

#### Reload bash into a new shell
```bash
exec bash
```

> ⚠️ **Be careful:** Your old bash 3 version would still be installed in `/bin/bash` while the brew version would be in `/usr/local/bin/bash` . You can check which version you are using with `echo $SHELL` or `which bash` . 

#### Check `bash` version 
```bash
$> bash --version
GNU bash, version 5.1.4(1)-release (x86_64-apple-darwin20.2.0)
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
```

#### Bash version not updated

If you do not see the updated version, your path environment variable might need some rework. 

Reload bash with full path into a new shell
```bash
exec $(echo $(brew --prefix)/bin/bash)  
```

#### Change default bash on macOS

First, update the list of permitted shell by adding the bash brew version into `/private/etc/shells`. 
```bash
$> echo $(brew --prefix)/bin/bash | sudo tee -a /private/etc/shells
/usr/local/bin/bash
```

To check `/usr/local/bin/bash` has been added to `/private/etc/shells`
```bash
$> cat /private/etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
/usr/local/bin/bash
```


Finally, update your user's shell with `chpass` command line. 
```bash
$> sudo chpass -s /usr/local/bin/bash alvin
Changing shell for alvin.
```

> Alternatively, instead of using `chpass` , you can go to the Menu > `System Preferences...` > `Users & Groups` . Unlock the pane, control click on your user to select `Advanced Options...` , then update the `Login shell` to `/usr/local/bin/bash`


## bash setting on macOS
For `bash`, you need to have the following files on macOS:

* `~/.bashrc` to initialize the bash interactive shell session. 
* `~/.bash_profile` to load the path variables 
* `~/.profile` for non-interactive shell session for commands



## Use correct command utilities program and documentation

There's a big differences in how a command is used based on either Linux (GNU) or macOS (BSD) version of the command utilities.  

`awk`, `grep`, `sed` in macOS most likely are BSD version. 

In most tutorial you find online including DataQuest are based on GNU command utilities. 

Regardless of whether you used BSD or GNU command utilities, ensure you use the right (BSD or GNU) documentation. 

## Install GNU core command line utilities on macOS 
[Guide to install GNU core utilities](https://apple.stackexchange.com/questions/69223/how-to-replace-mac-os-x-utilities-with-gnu-core-utilities): 
```
brew install coreutils findutils gnu-tar gnu-sed gawk gnutls gnu-indent gnu-getopt grep
``` 

## git setup

### Connecting to Github with ssh on MacOS 

0. [Configure ssh setup on macOS terminal](http://kbroman.org/github_tutorial/pages/first_time.html)

1. [Checking for existing SSH keys](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/checking-for-existing-ssh-keys)

2. [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

3. [Testing your ssh connection to github account](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/testing-your-ssh-connection)

4. [Using SSH authentication on terminal](https://gist.github.com/ddeveloperr/1859fd395e7cb5832c59)

To see your repo URL, run:
```bash
git remote show origin
```
https://github.com/username/reponame.git

To use ssh, you have to change repo URL, do the following:
```bash
git remote set-url origin git+ssh://git@github.com/username/reponame.git
```
## Do **NOT** install Python from python.org

It seems pretty obvious that you should install Python using the installer from Python’s official website.

The problem with this is that Python installs itself in a place that is difficult to manage without using administrator (i.e., sudo) privileges. 
This is okay for beginners, or people who only touch Python every so often. 
However, for my use cases, where I’m testing my code in multiple versions of Python, and have [multiple virtual environments](https://docs.python.org/3/tutorial/venv.html), this becomes bad news.

## Do **Not** install Python from `brew install python3`


## python setup 
This python setup guide use in part of the following:

- [A gist about python_environment.md](https://gist.github.com/wronk/a902185f5f8ed018263d828e1027009b#using-the-workflow)
- [Homebrew Documentation on Python](https://docs.brew.sh/Homebrew-and-Python)
- [Using pyenv to install and manage multiple python
  version](https://anil.io/blog/python/pyenv/using-pyenv-to-install-multiple-python-versions-tox/)
- [tox automation project](https://pypi.org/project/tox/)
- [tox documentation](https://tox.readthedocs.io/en/latest/)
- [virtualenv installation](https://virtualenv.pypa.io/en/stable/installation.html)


Follow the order of steps in order to setup Python.


## Python build dependencies

<a name="install_pyenv">For `pyenv` to install Python correctly, required to install the Python build
dependencies.</a>

1. Install [Python build dependencies](https://github.com/pyenv/pyenv/wiki#suggested-build-environment) before attempting to install a new Python
   version.

**Notes**: Highly recommend to install Python build dependencies before any
installation with Python and its related modules. 

```bash
# optional, but recommended:
brew install openssl readline sqlite3 xz zlib
```


## Install Python using `pyenv`
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
### Locating the Python Installation

Each Python version is installed into its own directory under `$(pyenv root)/versions`.

For example, you might have these versions installed:

- `$(pyenv root)/versions/2.7.8/`
- `$(pyenv root)/versions/3.9.1/`
- `$(pyenv root)/versions/pypy-2.4.0/`

As far as pyenv is concerned, version names are simply the directories in `$(pyenv root)/versions`.



### Uninstalling Python Versions 

As time goes on, you will accumulate Python versions in your `$(pyenv root)/versions` directory.

To remove old Python versions, `pyenv uninstall` command to automate the removal process.

Alternatively, simply `rm -rf` the directory of the version you want to remove. You can find the directory of a particular Python version with the `pyenv prefix` command, e.g. `pyenv prefix 2.6.8.`

### Upgrading pyenv 

If you've installed pyenv using homebrew, upgrade using:
```bash
brew upgrade pyenv
```

If you've installed pyenv using the instructions above, you can upgrade your installation at any time using git.

To upgrade to the latest development version of pyenv, use git pull:
```bash
cd $(pyenv root)
git pull
```
To upgrade to a specific release of pyenv, check out the corresponding tag:
```bash
cd $(pyenv root)
git fetch
git tag
git checkout v0.1.0
```


## Install Python package manager and setup tools 

### pip 
According to [pip manual](https://pip.pypa.io/en/stable/installing/#install-pip), pip is already installed if you are using Python 2 >=2.7.9 or PYthon 3 >= 3.4 downloaded from [python.org](python.org) or if you are working in a virtual environment created by virtualenv or venv. Just make sure to upgrade pip. 

Check if pip is already installed: 
```bash
python -m pip --version 
```


### Installing pip with get-pip.py

Download get-pip.py
```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

Install pip 
```bash
python get-pip.py
```

### Upgrade setuptools and pip

Setuptools can be updated via pip3, without having to re-brew Python 
```bash
python3 -m pip install --upgrade setuptools
```

pip3 can be used to upgrade itself via: 
```bash
python3 -m pip install --upgrade pip
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
### Install pipx 

`pipx` enables you to
- Expose CLI entrypoints of packages ("apps") installed to isolated environments with the install command. This guarantees no dependency conflicts and clean uninstalls!
- Easily list, upgrade, and uninstall packages that were installed with pipx
- Run the latest version of a Python application in a temporary environment with the `run` command
- Best of all, pipx runs with regular user permissions, never calling `sudo pip install` (you aren't doing that, are you? 😄).

On macOS:
```bash
brew install pipx 
pipx ensurepath 
```
And, upgrade `pipx` 

```bash
brew update && brew upgrade pipx
```

Otherwise, install via `pip`

```bath
python3 -m pip install --user pipx 
python3 -m pipx ensurepath
```

Upgrade `pipx` :
```bash
python3 -m pip install --user -U pipx
```

Shell completions are available by the following instructions printed with this
command.

```bash
pipx completions
```
### Install a package and its applications with pipx

You can globally install an application by running 
```bash
pipx install PACKAGE
```

This automatically creates a virtual environment, installs the package, and adds the package's associated applications (entry points) to a location on your PATH. 
For example, `pipx install pycowsay` makes the `pycowsay` command available globally, but sandboxes the pycowsay package in its own virtual environment. **`pipx` never needs to run as `sudo` to do this**.

## Setup Python Virtual Environment Settings

### [`pyenv`](https://github.com/pyenv/pyenv)
[`pyenv`](https://github.com/pyenv/pyenv) is required before installing `virtualenv`. <a href="#install_pyenv">See above installing
`pyenv`.</a>


### [`virtualenv`](https://virtualenv.pypa.io/en/stable/)
Short for "virtual environment." This manages separate directories for the modules you install (e.g., with `pip`). That way, each virtual environment can have it's own set of installed modules that is walled off from every other virtual environment so they don't interfere with each other. Like with `pyenv`, we'll switch virtual environments with virtualenvwrapper's `workon` command (as described later).



Install virtualenv. If no `pipx`, try `pip install virtualenv`
```bash
pipx install virtualenv
```

**Technical details:** `virtualenv` keeps each environment (and its installed modules) in separate folders; therefore, each is like a silo that doesn't interact with any other virtual environment. Usually, the exact file location is defined by the user, but we can use virtualenvwrapper to instead handle this for us.


### [`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.io/en/latest/) pyenv plugin to manage virtual environment 


This helps pyenv and virtualenv gel like PB&J. With it, you can effectively switch between a single environment that has both the Python version and virtual environment wrapped in one bundle. **Make sure `pyenv` and `virtualenv` are installed before you install this wrapper**.


Try using `pip` if no `pipx` 
```bash
pipx install virtualenvwrapper
```

`pyenv-virtualenvwrapper` is a `pyenv` plugin to manage your `virtualenvs` with
`virtualenvwrapper`. 

Then extend the usage of `pyenv` by install the `pyenv-virtualenvwrapper`:
```bash
brew update
brew install pyenv-virtualenvwrapper
```

`eval "$(pyenv virtualenv-init -)"` is configured in your shell
`~/.bash_profile`, `pyenv-virtualenv` will automatically activate/deactivate
virtualenvs on entering/leaving directories which contain a `.python-version`
file that contains the name of a valid virtual environment. `.python-version`
files are used `pyenv` to denote local Python versions and can be created and
deleted with `pyenv local` command. 


`virtualenvwrapper_lazy` [decrease shell time](https://www.reddit.com/r/Python/comments/11e773/tip_virtualenvwrapper_has_a_lazy_version_you_can/)

Append the following to `~/.bash_profile`:

```bash
export PYENV_VIRTUALENVWRAPPER_PREFER_PYVENV="true"
export WORKON_HOME=$HOME/.virtualenvs
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
pyenv virtualenvwrapper_lazy
```
Reload the shell:
```bash
source ~/.bash_profile
```

### Commmand usage

Using `mkvirtualenv` to make a virtual environment `venv_test`
```bash
mkvirtualenv venv_test
```

Switch between virtual environment using `deactivate` and `workon` commands. 
```bash
deactivate
workon venv_test
(ven_test) $ 
```

## Troubleshooting Python Environment setup issues

### 1. Error finding `virtualenvwrapper.hook_loader`

```bash
Error while finding spec for 'virtualenvwrapper.hook_loader' (<class 'ImportError'>: No module named 'virtualenvwrapper')
virtualenvwrapper.sh: There was a problem running the initialization hooks.
```
The solution also fixed cannot switch python version with `pyenv` using `pyenv
shell` command. 

The solution requires the understanding on [how pyenv works](https://mungingdata.com/python/how-pyenv-works-shims/). 

Stackoverflow reference: [1](https://stackoverflow.com/questions/33216679/usr-bin-python3-error-while-finding-spec-for-virtualenvwrapper-hook-loader)


Background to the problem: **`pyenv` changes `PATH`**
`pyenv` adds this code to the `~/.bash_profile` which changes the `PATH`.

```bash
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```
Run `echo $PATH` to see the `PATH` is different now: 
```bash
/Users/alvin/.pyenv/shims:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

`/Users/alvin/.pyenv/shims` has been added before all other directories in the
`PATH`. 

All terminal commands will go through `/Users/alvin/.pyenv/shims` first now.
This allows pyenv to "intercept" any relevant Python commands. 

- The `python` command will go to `/Users/alvin/.pyenv/shims/python`
- The `pip` command will go to `/Users/alvin/.pyenv/shims/pip`

**The solution uses the correct path to resolve the issues, and the solution are
follows**:
```bash
$> which -a python 
/Users/alvin/.pyenv/shims/python
/usr/bin/python
```
Select the first path which is `/Users/alvin/.pyenv/shims/python` or
`~/.pyenv/shims/python`. 

Add the following to `~/.bash_profile`:
```bash
export VIRTUALENVWRAPPER_PYTHON=/Users/alvin/.pyenv/shims/python3
```

Locate the path of `virtualenvwrapper.sh`
```bash
$> pyenv which virtualenvwrapper.sh 
/Users/alvin/.pyenv/versions/3.9.1/bin/virtualenvwrapper.sh
```
Add the following to `~/.bash_profile`:
```bash
source /Users/alvin/.local/bin/virtualenvwrapper.sh
```

The order of addition of fixes into `~/.bash_profile`
```bash
export PATH="$PATH:/Users/alvin/.local/bin"
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
export VIRTUALENVWRAPPER_PYTHON=/Users/alvin/.pyenv/shims/python3

# for virtualenv, Tell pyenv-virtualenvwrapper ti use pyenv when creating new Python environments
export PYENV_VIRTUALENVWRAPPER_PREFER_PYVENV="true"

# Setup virtualenv home
export WORKON_HOME=$HOME/.virtualenvs
source ~/.local/bin/virtualenvwrapper.sh

if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
eval "$(pyenv virtualenv-init -)"

pyenv virtualenvwrapper_lazy
```
### 2. On startup, macOS terminal fails to load `eval "$(pyenv init -)"`

`~/.bash_profile` is not sourced when terminal is start up. 

To fix, add the following to `~/.bashrc`:

```bash
if [ -f ~/.bash_profile ]; then
  . ~/.bash_profile
fi

```
## Workflow of Python Virtual Environment




Additional Resources
- [Real Python Primer on Virtual Environment](https://realpython.com/python-virtual-environments-a-primer/)
- [virtualenvwrapper Command List](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html)
- [pyenv Command Reference](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md)
- [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)
- [direnv](https://direnv.net)


## Node.js 

Install Node and `npm`
```bash
brew install node
```
> [More details on how to install NodeJS on macOS using homebrew](https://dyclassroom.com/howto-mac/how-to-install-nodejs-and-npm-on-mac-using-homebrew).


Check version of node
```bash
node -v
```

Check version of npm 
```bash
npm -v
```

Install `yarn` through homebrew package manager. 
```bash
brew install yarn
```

This will also install `Node.js` if it is not installed.

Check that `yarn` is installed by running
```bash
yarn --version
```

Make sure npm is up to date
```bash
sudo npm update -g npm
```
> Updates npm globally on your system. You may need `sudo` if you don't have
> permission. 


Install Bower - a package manager for the web
```bash
npm install -g bower
```
> `-g` will install `bower` globally in your system

Check version of bower
```bash
bower -v
```

Install Grunt 
```bash
npm install -g grunt-cli 
```
> `-g` will install `grunt` globally and put `grunt` in the system path


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


`/usr/bin/clang` failed with exit code 1 when installing psycopg2 
`xcrun: error: invalid active developer path /Libarary/Developer/CommandLine
Tools` - [Solution for error after macOS update](https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-pa)

Did the following on bash terminal 
```
xcode-select install 
sudo xcode-select --reset 
brew reinstall openssl 

```

[In venv](https://stackoverflow.com/questions/39767810/cant-install-psycopg2-package-through-pip-install-is-this-because-of-sierra/39800677#39800677)
```
env LDFLAGS='-L/usr/local/lib -L/usr/local/opt/openssl/lib -L/usr/local/opt/readline/lib' pip install psycopg2==2.8.4

```
[Other solution to install psycopg2](https://stackoverflow.com/questions/59206956/install-psycopg2-failed-on-macos)


