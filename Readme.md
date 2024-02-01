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

## Update:
I did not play with this a lot, but it looks like the following works with zshell: 
Add the following to ~/.zshrc:
```
export PYTHONPATH=$PYTHONPATH:/Users/maximeboudreau/Documents/git
```

To do this, you can type the following commands in the terminal:
```
cd ~
nano .zshrc
```
This will enter the nano editor. You can then add the path of you choice.

## Another way to play with the command names: 
If you navigate to this folder, you can find all aliases for different command and can rename them:
/usr/local/bin/

 
## PyODBC on a mac 
### Installing the odbc connector:
I used the installer found here:https://dev.mysql.com/downloads/connector/odbc/
Initally, I used the wrong version. This was the reason it was not working. You have to make sure you install the version corresponding to your mac architecture. 

### Succesfully installing and importing pyodbc on max 
https://stackoverflow.com/questions/66731036/unable-to-import-pyodbc-on-apple-silicon-symbol-not-found-sqlallochandle
This is what has worked for my Mac M1 Pro

### Registering the driver 
The following testing script can be used to assess if it work (you need to run a local database and update the connection script to match the credentials).

```

import pyodbc

class Connection:
    """
    Connects to the database for analysis and submission
    of results.
    """
    def __init__(self, databaseName="Maxime_TestDB" ,uid = None, pwd = None):
        if uid is None:
            uid = 'root'
            pwd = 'rootuser'
            connection_string = ('DRIVER=MySQL ODBC 8.2 ANSI Driver;'
                                 'SERVER=127.0.0.1:3306;'
                                 'DATABASE={0};'
                                 'UID={1};'
                                 'PWD={2};'
                                 'charset=utf8mb4;').format(databaseName, uid, pwd)
        self.connection = pyodbc.connect(connection_string)
    
        self.cursor = self.connection.cursor()
        self.WL_map = None
    def close(self):
        self.connection.close()

db = Connection(databaseName = 'test_schema')
```

When I tried this script, I had the following error:
```
pyodbc.Error: ('01000', "[01000] [unixODBC][Driver Manager]Can't open lib 'MySQL ODBC 8.2 ANSI Driver' : file not found (0) (SQLDriverConnect)")
```
Even though I had installed the ODBC driver, it was not being registered by the library. The problem is that it was looking for the following file: 
/Users/maximeboudreau/Library/ODBC/odbcinst.ini
Inside this file, the driver was not registered. I had to change the file to add the following information:
 ```
[ODBC Drivers]
MySQL ODBC 8.2 Unicode Driver = Installed
MySQL ODBC 8.2 ANSI Driver    = Installed

[ODBC Connection Pooling]
PerfMon    = 0
Retry Wait = 

[MySQL ODBC 8.2 Unicode Driver]
Driver = /usr/local/mysql-connector-odbc-8.2.0-macos13-arm64/lib/libmyodbc8w.so

[MySQL ODBC 8.2 ANSI Driver]
Driver = /usr/local/mysql-connector-odbc-8.2.0-macos13-arm64/lib/libmyodbc8a.so

```
Another potentially useful tool was the ODBC Driver manager: 
https://www.iodbc.org/dataspace/doc/iodbc/wiki/iodbcWiki/Downloads


### Running VSCODE Python scripts in bash instead of zshell:
<img width="647" alt="image" src="https://github.com/MxBoud/MacPythonConfig/assets/21281558/732361eb-9691-4d42-aa28-d192d1a6a22b">

Command + Shift + P and then :Select Default Profile. Then you have to select Bash,
