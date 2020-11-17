# CAETÊ
This is the implementation of the Dynamic version of CAETÊ - including Nitrogen and Phosphorus cycling

The code in this repository is based on an earlier version of CAETÊ that was not dynamic and did not have the N and P cycling implemented.

THis is part of my ongoing PhD research project.

# Development Dependencies
CAETÊ depends on a few packages that must be installed beforehand.
You can installed them using your favorite package manager: `apt`, `brew`, `pip`, `poetry`, etc.

General Dependencies
- make (for building automation)

Python Dependencies
- numpy
- f2py (part of numpy)
- cftime
- pandas
- matplotlib
- ipython

Make sure you have them properly installed before running the code.

# Running and Developing CAETÊ
**his section suposses you have a working python environment and an installed fortran compiler. If you need help setting up your invornment, check this link.**

CAETÊ uses both Python and Fortran and uses `f2py` module to create an interface between them. This means that the Fortran code must be compiled before you can run Python code.

The Makefile inside `/src` folder have useful automation to make it easier.

`make clean` - it clear your python cache, deletes the `/output` folder and deletes all compiled fortran files, including the `.pyf` file.

`make so` - compiles all fortran code and creates the `caete_module.pyf`, the interface between Fortran and Python code.

To run CAETÊ you can do the following:
```bash
# Clean you cache
make clean

# Build caete_module.pyf
make so

# Run the model
python model_driver.py
```

You can also run it interactively inside `ipython` to have direct access to the variables or you can also run it directly from the vscode debug environment. To run using vscode debug environment follow this little guide (Running CAETÊ from vscode debug environment)[#Development-Environment]

# Development Environment
These are some general directions to install a sane `python` development environment so you can avoid multiple python version conflict. This will avoid a lot of `sudo` problems for example or running a library with the wrong `python` version.

In this guide you will install:
* pyenv to manage python version.
* Python
* (Optionally) Poetry to manage the project dependencies.
* Vscode extensions for debuging python and fortran90.

## Installing Python
The solution here is to use a `python` version manager called `pyenv`. It will enable you to configure a python environment without breaking the python environment of your operating system.
This is largely based on this great tutorial: [Creating a sane Python Dev Env](https://pwal.ch/posts/2019-11-10-sane-python-environment-2020-part-1-isolation/)


## Pre-requesits

**For MacOS**

For MacOs you need to install Xcode, Xcode Command Line Tools and Homebrew.
XCode:
To install Xcode, go to the App Store and download it. It is a large download ~(12GB) so it can take a while.
Then, open Xcode and install any additional component if it requires any.
After installing Xcode, restart your Mac.

Xcode Command Line Tools
Open your terminal and paste the following:
```
xcode-select --install
```
It may open you update center or say you already have command line tools installed.
Close your terminal and open it again so changes can have effect.

Homebrew
Homebrew is a packege manager for MacOs, it works much like `apt` in linux systems. To install it, just paste the code in your terminal:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
Close your terminal and open it again so changes can have effect.

Support Libraries
Now you can install the dependencies for `pyenv`, for that just run on your terminal:
```
brew install openssl readline xz
```
Close your terminal and open it again so changes can have effect.
And now you are ready to install `pyenv`.

**For Linux (Ubuntu)**

To install the required dependencies on Ubuntu you can simply use `apt-get`. Just run the following:
```
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```

Close your terminal and open it again so changes can have effect.
And now you are ready to install `pyenv`.

## Installing and configuring pyenv
`pyenv` can be installed by a simple command line, just open your terminal and paste:
```
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```
Close your terminal and open it again so changes can have effect.

Now you need to add some initialization configuration.

If you are on MacOs, open your `~/.zshrc` file. You can open it in vscode using your terminal, just paste:
```
code ~/.zshrc
```

If you are on Ubuntu, open your `~/.bashrc` file. You can open it in vscode using your terminal, just paste:
```
code ~/.bashrc
```

At the end of the file, paste the following:
```bash
## Setting up shims for pyenv
# ref https://pwal.ch/posts/2019-11-10-sane-python-environment-2020-part-2-pyenv/
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
Close your terminal and open it again so changes can have effect.

to check if `pyenv` is working, just type on your terminal:
```
pyenv versions
```
You should see something like that:

```
➜ pyenv versions
* system (set by /home/fmammoli/.pyenv/version)
```
This is your system pyton, the one that comes installed by default by your MacOS or your Linux.

## Using Pyenv to install Python
Now we can finally install `Python` using `pyenv`.

For that, open your terminal and type:

```
pyenv install --list | grep 3.8
```
This will list all available 3.8 python versions, to look for other versions just change the 3.8 to 2.7, for example, if you need to install python 2 for some reason.

The version I want is 3.8.6, but find which version you need and then type:
```
pyenv install 3.8.6
```

This can take a while, so don't panic. If nothing changes in 10min or so, you may have a problem.

You should see something like that on your terminal, but with different filepaths:
```
Downloading Python-3.8.6.tar.xz...
-> https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tar.xz
Installing Python-3.8.6...
Installed Python-3.8.6 to /User/fmammoli/.pyenv/versions/3.8.1
```

Update your configurantions by typing:
```
pyenv rehash
```

To check if your new python version was installed correctly, just type:
```
pyenv version
```
And you should see:
```
* system (set by /User/fmammoli/.pyenv/version)
  3.8.6
```
The * indicates your active Python.

### Changing your default python version

You can change your python version using the `global` command, like the following:
```
pyenv global 3.8.6
```
This will set the python 3.8.6 (the one you just installed previously) as your global pyton version.
To guarantee the changes have effect, close your terminal and open it again.

Now you can run `pyenv versions` on your terminal and you should see something like:
```
 system 
* 3.8.6 (set by /User/fmammoli/.pyenv/version)
```
The * indicates your active python version.

To run `python` on your terminal, you just type:
`python`
And the intercative prompt should start with your active python version. You should see something like:
```bash
Python 3.8.6 (default, Oct 20 2020, 15:15:44)
[Clang 12.0.0 (clang-1200.0.32.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```

You can change your global python to other versions you can use the same syntax, say you want to use the original system version:
```
pyenv global system
```

And that is it! Now you have a 

## Installing Poetry
`Poetry` works as a virtual environment manager and as a dependence manager for `python` projects. By using `poetry` we can guarantee that everyone that cloned the repository is using the same `python` version and the same version for every library. This reduces inconsistencies among different environments making bugs easier to track.

To install `poetry` you just need to run a simple script on your terminal.
So open a new terminal and just paste the following:
```
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```
To check if the installation was successful, open a new terminal and run:
```
poetry --version
```
You should see something like `Poetry version 1.1.4`. If not refer to the `poetry` installation guide.

By default, `poetry` install the virtual environment (venvs) in the user folder, which is quite obscure, but we can change it so it installas everything inside the project folder in a new `.venv` folder. For that just run on your terminal:
```
poetry config settings.virtualenvs.in-project true
```

Now you can use `poetry` to stop suffering from weird environment problems.

## Configuring CAETE-DVM to run on poetry
To use 

## Installing Dependencies for the CAETE-DVM
Dependencies for the python project are described in the `pyproject.toml`.
Once you clone the repository you have to install all its dependencies.

Since we are using `poetry`, this sted is very simple. First open the terminal insde vscode and `cd` to the root folder of the CAETE-DVM if you are not in it.

Now we have to start the `poetry` virtual environment, for that just run:
```
poetry shell
```
This will create a new shell with the python described in `pyproject.toml` and tell vscode to use the virtual env interpreter.

Now, to install the dependencies, just run:
```
poetry install
```
This will download and install all the required dependencies locally, this can take some time, about 5 to 10min depending on your internet connection. Is it is hanged for more then 3min in the same step, you probably have a problem. Just `ctrl + c` to stop the process and then run it again.

If you have no `error` on your terminal you have successfully installed all CAETE-DVM dependecies!

## Developing and Running the Model

## VS_Code Configuration

A configuration so you can debug the model code using vscode debug features and use both Python and Fortran Intellisense.

### Pre-requisites
This assumes you already have a fortran compiler installed, specially gfortran and a working python environment. If you don't, please read the tutorial from the start.

If you are running a Linux you problably already have gcc.

If you are running MacOS, gcc is installed when you install Xcode and the command-line-tool. (To see how to install both of these, refer to the start of the tutorial).

To check if you have gcc install on any platform, open your terminal and type:
```
gcc --version
```
You should see something like:
```bash
Apple clang version 12.0.0 (clang-1200.0.32.21)
Target: x86_64-apple-darwin20.1.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bi
```
If nothing appears, you may have a problem.

### Installing vscode extensions

First, we need to install the fortran-language-server. For that, open your terminal and run the following:
```
pip install fortran-language-server
```
Now open the CAETE project with vscode.

Now we can start installing some useful vscode extensions
For that click in the extension tab, inside vscode and install the following:
* Python, 
* Pylance, 
* Visual Studio Intellisense,
* Path IntelliSense,
* Modern Fortran*
* Fortran IntelliSense*

*Both Modern Fortran and Fortran IntelliSense depend on an already installed `gfortran` compiller, and Fortran IntelliSense needs the Fortran Language Server running, referer to their offical installation tutorial in case of problems.

### Configuring your vscode environment
Now let's configure everything.

In the root folder of your project you will find the `/.vscode` folder, here is where all configurations will go.

Let's start with `setting.json` file.

First check if if the project is using the correct `python` version, 

If you are using  `poetry` to manage our virtual environemnts locally we can just point it to ou `.venv` folder. Add the following line:
```json
"python.pythonPath": ".venv/bin/python3.8",
```

If you are not using poetry, add the following:
```json
"python.pythonPath": "~/.pyenv/versions/3.8.6/bin/python",
```
This will inform vscode to use the python interpreter version 3.8.6, installed using pyenv. This line simply point to the python path.

Then, lets configure the new `pylance` language server. Add the following line:

```json
"python.languageServer": "Pylance",
```

Now let's configure the Fortran IntelliSense extention, add the following line:
```json
    "[fortran]": {
        "editor.acceptSuggestionOnEnter": "off"
    },
    "[fortran_fixed-form]": {
        "editor.acceptSuggestionOnEnter": "off"
    },
    "[FortranFreeForm]": {
        "editor.acceptSuggestionOnEnter": "off"
    }
```
In the end, your `settings.json` should look like this:
````json
{
    "python.pythonPath": ".venv/bin/python3.8",
    "python.languageServer": "Pylance",
    "[fortran]": {
        "editor.acceptSuggestionOnEnter": "off"
    },
    "[fortran_fixed-form]": {
        "editor.acceptSuggestionOnEnter": "off"
    },
    "[FortranFreeForm]": {
        "editor.acceptSuggestionOnEnter": "off"
    }
}
````

This will give you code completition and highlight for both Python and Fortran code.

Now let's configure the `lunch.json`. This file will be used by the vscode debug session to lunch the `model_driver.py` automatically.

Create the `lunch.json` inside `.vscode` folder if it does not exist create it and add the following:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Model Driver",
            "type": "python",
            "request": "launch",
            "cwd":"${workspaceFolder}/src",
            //"program": "${file}",
            "program": "${workspaceFolder}/src/model_driver.py",
            "console": "integratedTerminal"
        }
    ]
}
```
What it does is to inform vscode debug to run `model_driver.py` using python.

Now you run the model directly via the debug tab, use breakpoint, use the debug console inside vscode and watch variables.
You can ever run the model line by line checking all changes in any variables!

## Installing CAETÊ dependencies
The model depends on a few packages to run:
- numpy = "^1.19.4"
- cftime = "^1.2.1"
- pandas = "^1.1.4"
- matplotlib = "^3.3.3"
- ipython = "^7.19.0"

If you are using `poetry` as your python dependency manager, you just need to open the terminal inside vscode and type:
```
poetry install
```
This will install all the dependencies with their correct version using the `poetry.lock` and `pyproject.toml` as the reference files.

If you are not using poetry, you can install them using `pip` directly to your active python. It is better to install them one by one, so you can have a more precisa error log if something goes wrong. To install them just run each `pip` command on your terminal:
```bash
# To install numpy
pip install numpy

# To install 
pip install cftime

# To install pandas
pip install pandas

#To install matplotlib
pip install matplotpib

#To install ipython
pip install ipython
```

If you want to run the files inside the  `/input` folder, you will need different dependencies that are more complicated to install. This will be covered in a different tutorial.


## Running and Developing CAETÊ

CAETE uses fortran code that must be compiled beforehand so it can be used by the Python code.
To create this interface, CAETE uses `f2py` module that is located inside `numpy`.
All procedures to compile and interface the fortran code is managed in the `Makefile` inside `/src`, there you can change how you call the f2py module and check the individual steps.

Makefile provides a useful set of feature:
`make clean` will delete the outputs and cleand the python cache.
`make so` will compile the fortran code and run f2py.

To run the model you can simply do the following.
In your terminal, navigate to the `/src` folder:
````
cd /src
```

Then clean your cache and the old outputs by running:
```
make clean
```

Then compile the fortran code and create the python interface:
```
make so
```

To run the model, simply run:
```
python model_driver.py
```
This will take a long to time to run and use a lot of your computer resources, but you can stop at any time by pressing ctrl+c on your terminal.

All outputs will be saved on the folder `/src/outputs`.
To run the model again make sure you cleaned your previous outups by running `make clean`.
If you make any changes to any fortran file, you will need to run `make so` again, before running `model_driver.py`.

You can also run it through vscode debug tab. Considering you have already configured `.vscode/launch.json`, just go to the debug tab and click the little green arrow or press F5.

You can also run it interactively by using `ipython`.


python 

## __AUTHORS__:

 - Bianca Rius
 - David Lapola
 - Helena Alves
 - João Paulo Darela Filho
 - Put you name here! (labterra@unicamp.br)
