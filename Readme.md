# Configuring the good python version with PYTHONPATH environment variables on python: 

In the home directory, create a .bash_profile file with path being created in a similar way:
```
PATH= "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3:${PATH}"
export PATH

#PYTHONPATH = "/Users/maximeboudreau/Documents/Python:${PYTHONPATH}"
#export PYTHONPATH
export PYTHONPATH=$PYTHONPATH:/Users/maximeboudreau/Documents/Python


alias python=python3
```
Note! For this to work, you have to set the mac terminal to use bash and not zsh (in the settings!).
