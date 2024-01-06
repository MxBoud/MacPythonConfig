# Configuring the good python version with PYTHONPATH environment variables on python: 

In the home directory, create a .bash_profile file with path being created in a similar way:
```
PATH= "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3:${PATH}"
export PATH

#PYTHONPATH = "/Users/maximeboudreau/Documents/Python:${PYTHONPATH}"
#export PYTHONPATH
export PYTHONPATH=$PYTHONPATH:/Users/maximeboudreau/Documents/Python
#Adding homebrew to the path
eval "$(/opt/homebrew/bin/brew shellenv)"

alias python=python3
```
Note! For this to work, you have to set the mac terminal to use bash and not zsh (in the settings!).

## Another way to play with the command names: 
If you navigate to this folder, you can find all aliases for different command and can rename them:
/usr/local/bin/

 
## PyODBC on a mac 
https://stackoverflow.com/questions/66731036/unable-to-import-pyodbc-on-apple-silicon-symbol-not-found-sqlallochandle
This is what has worked for my Mac M1 Pro
