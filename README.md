# CAETE-Tutorials

Tutorials for running the CAETÊ model.

This is a collection of tutorials to run the [CAETÊ-DVM](https://github.com/jpdarela/CAETE-DVM) model. These are inteded to be used alongside the CAETE-DVM repository.

These guides are intended for MacOS or Linux, I have not tested it on Windows.

Here you will find guides for setting up a sane python environment, configuring your vscode, etc.

Inside the `/Example Files` you will finde some configurations files that can serve as base for your own.

To manage our Python we will use `pyenv`.

If you do not have python installed yet, are unhappy with your current install or are having problems, these guides are for you.

The recommended order for the tutorials is:

1. [Configuring our Development Environment](#configuring-your-development-environment)
2. [Installing and configuring pyenv](#installing-and-configuring-pyenv)
3. [Installing python using pyenv](#installing-python-using-pyenv)
4. [Using pyenv to manage your Python versions](#using-pyenv-to-manage-your-ython-versions)
5. [Configuring your VScode to debug CAETÊ](#configuring-your-VScode-to-debug-CAETE)
6. [Configuring VScode to debug Fortran code](#configuring-VScode-to-debug-Fortran-code)
7. [Installing CAETÊ dependencies](#installing-CAETÊ-dependencies)
8. [Running and Developing CAETÊ](#running-and-Developing-CAETÊ)
9. [A very minimal Git cheatsheet](#a-very-minimun-git-cheatsheet)

# Configuring your Development Environment

These are some general directions to install a sane `python` development environment so you can avoid multiple python version conflict. This will avoid a lot of `sudo` problems for example or running a library with the wrong `python` version.

In this guide you will install:

- pyenv to manage python version.
- Python
- (Optionally) Poetry to manage the project dependencies.
- Vscode extensions for debuging python and fortran90.

This is largely based on this great tutorial: [Creating a sane and modern Python Dev Env](https://pwal.ch/posts/2019-11-10-sane-python-environment-2020-part-1-isolation/)

## Pre requesits

### For MacOS

For **MacOs** you need to install Xcode, Xcode Command Line Tools and Homebrew.

**XCode:**
To install Xcode, go to the App Store and download it. It is a large download ~(12GB) so it can take a while.
Then, open Xcode and install any additional component if it requires any.
After installing Xcode, restart your Mac.

**Xcode Command Line Tools:**
Open your terminal and paste the following:

```
xcode-select --install
```

It may open you update center or say you already have Command Line Tools installed.
Close your terminal and open it again so changes can have effect.

**Homebrew:**
Homebrew is a packege manager for MacOs, it works much like `apt` in linux systems. To install it, just paste the code in your terminal:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Close your terminal and open it again so changes can have effect.

**Support Libraries:**
Now you can install the dependencies for `pyenv`, for that just run on your terminal:

```
brew install openssl readline xz
```

Close your terminal and open it again so changes can have effect.
And now you are ready to install `pyenv`.

### For Linux (Ubuntu)

To install the required dependencies on Ubuntu you can simply use `apt-get`. Just run the following:

```
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```

Close your terminal and open it again so changes can have effect.
And now you are ready to install `pyenv`.

# Installing and configuring pyenv

`pyenv` can be installed by a simple command line, just open your terminal and paste:

```
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```

Close your terminal and open it again so changes can have effect.

Now you need to add some initialization configuration.

If you are on **MacOs**, open your `~/.zshrc` file. You can open it in vscode using your terminal, just paste:

```
code ~/.zshrc
```

If you are on **Ubuntu**, open your `~/.bashrc` file. You can open it in vscode using your terminal, just paste:

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

Save and close the file.

Close your terminal and open it again so changes can have effect.

To check if `pyenv` is working, just type on your terminal:

```
pyenv versions
```

You should see something like that:

```bach
➜ pyenv versions
* system (set by /home/fmammoli/.pyenv/version)
```

This is your system Pyton, the one that comes installed by default by your **MacOS** or your **Linux**.

# Installing Python using Pyenv

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

The \* indicates your active Python.

### Using pyenv to manage your Python versions

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

The \* indicates your active python version.

To run `python` on your terminal, you just type:
`python`
And the intercative prompt should start with your active python version. You should see something like:

```bash
Python 3.8.6 (default, Oct 20 2020, 15:15:44)
[Clang 12.0.0 (clang-1200.0.32.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```

Now you have an isolated python installation! Congrats!

## Basic Usage

If you want to change your global python to other versions for any reason, you can use the same syntax, say you want to use the original system version:

```
pyenv global system
```

# Configuring your VScode to debug CAETE

## VScode Configuration

A configuration so you can debug the model code using vscode debug features and use both Python and Fortran Intellisense.
You can refer to the file `/Example Files/.settings.json` and `/Example Files/.launch.json`

### Pre-requisites

This assumes you already have a fortran compiler installed, specially gfortran and a working Python environment. If you don't, please read the tutorial from the start.

If you are running a **Linux** you problably already have gcc.

If you are running **MacOS**, gcc is installed when you install Xcode and the command-line-tool. (To see how to install both of these, refer to the start of the tutorial).

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

## Installing vscode extensions

First, we need to install the fortran-language-server. For that, open your terminal and run the following:

```
pip install fortran-language-server
```

Now open the CAETE project with vscode.

Now we can start installing some useful vscode extensions
For that click in the extension tab inside vscode, search and install the following:

- Python,
- Pylance,
- Visual Studio IntellCode,
- Path IntelliSense,
- Modern Fortran\*
- Fortran IntelliSense\*

\*Both Modern Fortran and Fortran IntelliSense depend on an already installed `gfortran` compiller, and Fortran IntelliSense needs the Fortran Language Server running, referer to their offical installation tutorial in case of problems.

## Configuring your vscode environment

Now let's configure everything.

In the root folder of your project you will find the `/.vscode` folder, if you don't have one just create a new folder and name it `.vscode`. All configuration files will go inside of it.

Let's start with `settings.json` file.

First check if if the project is using the correct `python` version,

If you are using `poetry` to manage our virtual environemnts locally we can just point it to ou `.venv` folder. Add the following line:

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

```JSON
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

```JSON
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
```

You can see this file inside the `/Example Files` folder.

This will give you code completition and highlight for both Python and Fortran code.

Now let's configure the `launch.json`. This file will be used by the vscode debug session to launch the `model_driver.py` automatically.

Create the `launch.json` inside `.vscode` folder if it does not exist create it and add the following:

```JSON
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Model Driver",
            "type": "python",
            "request": "launch",
            "cwd":"${workspaceFolder}/src",
            "program": "${workspaceFolder}/src/model_driver.py",
            "console": "integratedTerminal"
        }
    ]
}
```

You can see this file inside the `/Example Files` folder.

What it does is to inform vscode debug to run `model_driver.py` using python.

Now you run the model directly via the debug tab, use breakpoint, use the debug console inside vscode and watch variables.

You can ever run the model line by line checking all changes in any variables!

# Configuring VScode to debug Fortran code

To debug fortran code in fscode with breakpoint you will need `gdb`, the `Modern Fortran` vscode extension,the `Fortran Breakpoint Support` vscode extension, and `C/C++` vscode extintion.
You can find all of them in the vscode extention marketplace

If you are following this tutorial from the begining you have already `Modern Fortran installed`, and if you are ussing linux, you already have `gdb`.

Make sure to have them all those extension install them all before continuing.

After that you need to create a vscode task to compile your fortran code, you can use this as an example:
Create a file named `tasks.json` insede your `.vscode` folder:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "make debug",
      "type": "shell",
      "command": "make debug",
      "options": {
        "cwd": "${workspaceRoot}/src"
      }
    }
  ]
}
```

This task simply runs the `make debug` inside the Makefile. The `make debug` compiles all fortran files into a library to be used in another fortran program, it compiles using `gfortran` instead of F2PY. Since the fortran modules are compiled into a library, they can be tested using another fortran program, such as the `debug_caete.f90`.

Then, you need to configure how the vscode debugger will be lounched.
For that open you `launch.json` inside you `.vscode` folder and add the following:

```json
{
  "name": "Debug Fortran",
  "type": "cppdbg",
  "request": "launch",
  "program": "${workspaceRoot}/src/run_debug",
  "args": [],
  "stopAtEntry": false,
  "cwd": "${workspaceRoot}",
  "externalConsole": false,
  //If you are using linux you probably already have gdb
  "MIMode": "gdb",

  //If you are using macOS you probably have lldb, which is worst than gdb.
  //"MIMode": "lldb",
  //"miDebuggerPath": "/usr/bin/lldb",

  "preLaunchTask": "make debug"
  //You can uncomment the loggin options in case you are having trouble
  // "logging": {
  //   "trace": true,
  //   "traceResponse": true,
  //   "engineLogging": true
  // }
}
```

If you are using linux you already have `gdb`, so you don need to change anything in the code above.
What this configuration does is: it run the `make debug` task we have previously configured in our `tasks.json` file and then runs the `run_debug.f90` file that is inside the `/src` folder, in the vscode debugger interface.

With that you can set breakpoints in your fortran code, check stacktraces and variables values using the vscode debug interface.

Since `caete_module` is compiled as a library and not a program, you will need to make a program that calls specific subroutines in the `caete_module` file. For an example of how to do it check the `debug_caete.f90` file, inside the `/src` folder.

# Debugging Mixed Python and Fortran code

I am still working on a mixing debuggin solutiion. I haven't managed to make it work in anyway and I don't think it has any simple solution. Solutions I have found use paid debugger aplication or a paid IDE, such as the Microsoft Visual Studio or the ARM debugger.

I think it is probably an f2py limitation.
I tried to run the model_drive.py using the Python debugger and while it is on pause I attached `lldb` to the python process running the `model_driver.py`. I added a breakpoint to the `budget.f90`it does not pause on it. I am not sure if there is a reasonable solution to that.

I can compile the fortran code with f2py using debug flags like so:

```makefile
interface: $(sources)
	echo "Running interface"
	$(F2PY) $(HFLAG) --debug  $(INTERFACES) $(sources) $(MFLAG) --debug-capi $(MODNAME) $(OVRTFLAG) --quiet

so: $(sources) interface
	echo "Running so"
	$(F2PY)  $(INTERFACES) $(CFLAG) --debug --debug-capi  $(sources) --f90flags="${FCFLAGS} -Wall $(EXT_FLAGS) -fopenmp " -lgomp
	echo

```

The best I got was: I run `model_driver.py` in the debugger with a breakpoint somewhere, when it stops I attach a lldb debugger to the python process. If I manually stop the fortran debugger it stops the code execution but only shows me some assembly code, not the fortran lines...not sure what to do.

If anyone has an idea, please tell me.
This is the best post I found about it:
(https://nadiah.org/2020/03/01/example-debug-mixed-python-c-in-visual-studio-code/)[https://nadiah.org/2020/03/01/example-debug-mixed-python-c-in-visual-studio-code/]

# Installing CAETÊ dependencies

CAETÊ depends on a few packages to run:

- numpy
- f2py (part of numpy)
- cftime
- pandas
- matplotlib
- ipython

First check if your active python is the one you have just installed and not the system one:

```bash
# Type on your terminal
pyenv version

# You should see something like this.
 system
* 3.8.6 (set by /User/fmammoli/.pyenv/version)
```

You can install all of them in your active python by using `pip`, the python package. It is safer to install each one individually so you can have a more precise log if something goes wrong.

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

If you want to run the files inside the `/input` folder, you will need different dependencies that are more complicated to install. This will be covered in a future tutorial.

# Running and Developing CAETÊ

CAETE uses fortran code that must be compiled beforehand so it can be used by the Python code.
To create this interface, CAETE uses the `f2py` module that is located inside `numpy`.
All procedures to compile and interface the fortran code is managed in the `Makefile` inside `/src`, there you can change how you call the f2py module and check the individual steps.

Open the Makefile on your CAETE-DVM project, locate the F2PY variable, it is the first one, and adjust the python call to your liking. If you follow this tutorial you can set as:

```makefile
F2PY = python -m numpy.f2py
```

CAETÊ uses both Python and Fortran and uses `f2py` module to create an interface between them. This means that the Fortran code must be compiled before you can run Python code.

The Makefile inside `/src` folder have useful automation to make it easier.

`make clean` - it clears your python cache, deletes the `/output` folder and deletes all compiled fortran files, including the `.pyf` file.

`make so` - compiles all fortran code and creates the `caete_module.pyf`, the interface between Fortran and Python code.

To run CAETÊ you can do the following:

```bash
# Navigate to your /src folder.
cd /src

# Clear you cache.
make clean

# Compile the fortran files and build caete_module.pyf interface.
make so

# Run the model.
python model_driver.py
```

All mesages will be printed on your terminal, you can run CAETÊ from the vscode terminal too if you wish so.

This will take a long to time to run and use a lot of your computer resources, but you can stop at any time by pressing `ctrl+c` on your terminal.

All outputs will be saved on the folder `/src/outputs`.
To run the model again make sure you cleaned your previous outups by running `make clean`.
If you make any changes to any fortran file, you will need to run `make so` again, before running `model_driver.py`.

You can also run it interactively inside `ipython` to have direct access to the variables or you can also run it directly from the vscode debug environment. To run using vscode debug environment follow the previouse guide.

# A very minimun git cheatsheet

These are some very basic git command in case you have forgotten them.

To clone a repository to your computer:
`git clone link_to_the_repository_`

To list all your branches:
`git branch`

The `*` will show which branch you are currently on.

To create a new branch: (avoid using spaces, git does not deal well with it, so use - or \_ between words)
`git branch my_branch`

After you crate a branch you probably want to move to it, to do it just type:
`git checkout my_branch`

Now you are in your new branch.

Then you changes some lines, to see what files have been changed:

`git status`

You realised you did not liked the changes you made to one specific, and you want to go back to the original file. You can just do: (Pay attention to the double slashes before the name of the file!!!)
`git checkout --the_file_you_messed_up`

I another file, you have changed some stuff and you liked it! and now you want to commit it. Simply do:

`git add the_name_of_the_file_that_you_changed`

To all all the files you have changes you can simply:

`git add .`

This is dangerous, try to avoid it. It is much better to add file by file.

Now you have added all your amazing changes and you can commit them to your branch.
Before commiting it is always useful to double check the branch you are in with `git branch`, you don't want to commit stuff in the wrong branch.

Now that you are sure you are in the right branch, you can commit by doing:
`git commit -m "you message"`
It is good practice to write a good short description of you change, and remember that the text must be inside the double quotes " ".

You realized you commits were great and you want to add it to you master. The best way to do it is to make a pull request. For that you just need to push your personal branch to github. Simply do it like:
`git push origin my_branch`

Now you can go to your gihub and open a pull request

While you were working on you branch you realized that the master branch has been updated.
I you want to update you master you must, move to the master branch and pull all the up updates from the remote master. You can do this by:
`git checkout master` -> to move to the master branch
`git pull origin master` -> this will get the most updated version of the master branch in add it to your local master branch.

If you want to add the changes you just received on you master to your personal branch, you need to move back to your branch and merge your local master to your branch. This is a bit dangerous, so be careful:
`git checkout my_branch` -> to move to the branch you created earlier
`git merge master` -> this will apply the updated master branch to your branch

To check the url of you `origin` you can simply:
`git remote -v`

An that is basically it.

## Keeping your fork up to date

First you need to add the original repository into your remotes. We usually call the original repository upstream.
To add to that in your terminal:
`git remote add upstream https://github.com/jpdarela/CAETE-DVM.git`

now if you run `git remote -v` on your terminal to list your remotes you should see something like:

```bash
origin https://github.com/fmammoli/CAETE-DVM.git (fetch)
origin https://github.com/fmammoli/CAETE-DVM.git (push)
upstream https://github.com/jpdarela/CAETE-DVM.git (fetch)
upstream https://github.com/jpdarela/CAETE-DVM.git (push)
```

Your `origin` is the master branch on your fork, and `upstream` is the original repository that your forked from.

Now we will pull the changes in the `upstream` to our master branch.
For that make sure your are in your master branch.
`git checkout master`

than you can just
`git pull upstream/master master`

that just mean you will take the code from the master branch on the `upstream` repository and apply the changes into your local master branch.

To send the this new updated master to your fork master, namely, your origin, just make a push
`git push origin master`

That means your as sending the contents of your master branch into the origin.

And you are done! :)

# **AUTHORS**:

- Bianca Rius
- David Lapola
- Helena Alves
- João Paulo Darela Filho
- Put you name here! (labterra@unicamp.br)
