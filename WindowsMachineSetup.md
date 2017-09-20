# Windows Developer Machine Setup
This document describes how to set up Windows developer box.


## Table Of Contents
* [Overview](#user-content-overview)
* [Windows Machine Setup](#user-content-window-machine-setup)
	* [Install VSCode](#user-content-install-vscode) 
	* [VSCode Setup](#user-content-vscode-setup)
		* [VSCode for Windows 7](#user-content-vscode-for-windows-7) 
		* [VSCode for Windows 10](#user-content-vscode-for-windows-10) 
		* [VSCode Native Unit Test Debugging](#user-content-vscode-native-unit-test-debugging)
* [NVM Setup](#user-content-nvm-setup)


## Overview
This document describes how to set up a developer machine for UI development. Because we are leveraging bash commands for build, test, and run scripts, and some developer machines do not necessarily natively support this shell, i.e Windows, this document descibes how to set up a machine so that a developer can semelessly contribute value to UI development on these otherwise incompatable machines.


## Windows Machine Setup
This section describes how to set up a Windows machine in preparation for gabbing the UI project and starting development. There are two different setups depending upon what OS the developer machine is running. There are instructions for [Windows 10](#user-content-windows-10), which natively supports running a version of Linux, and instructions for [Windows <10](#user-content-windows-<10), which does not natively support running Linux. You should follow the instructions for your specific OS.

#### Windows 7 (Cygwin)
1. [Install Cygwin](https://www.cygwin.com/) 
	* Install all defaults (no action necessary)
	* Install packages 
		
		_(*Note: you will, unintuativley, click "Skip" to install a package)_
		
	* Add `ssh` package
		* Search for `ssh` _(Do Not Press Enter)_
		* Expand `NET`
		* Click "Skip" on `openssh`
	
	* Add vim 	
		* Search for `vim` _(Do Not Press Enter)_
		* Expand `Editors` 
		* Click "Skip" on `vim-common: Vi IMproved`

	* Add vim 	
		* Search for `chere` _(Do Not Press Enter)_
		* Expand `Shells` 
		* Click "Skip" on `chere`

	* Click "Next"
	* Click "Next" on the following screen "Resolving Dependencies"
	* _**Download Should Start**_
	* Choose your icon preferences _(this is completely subjective)_
	* Click Finish

2. Confirm Successful Installation
	* Open Cygwin
	* Type `cd /cygdrive/c/`
	* Type `ls`
	* You should see a list of your C:\ drive. Just as if you had run `dir` from command.

#### Window 10 (Windows Subsystem for Linux)
To install Windows Subsystem For Linux you will need to [Turn On Developer Mode]() and [Enable Windows Subsystem For Linux]().

##### Turn On Developer Mode
1. Open `Settings -> Update and Security -> For developers`
2. Select the Developer Mode radio button

##### Enable Windows Subsystem For Linux
1. From Start, search for "Turn Windows features on or off" (type 'turn')
2. Select Windows Subsystem for Linux (beta)
3. Click "OK"
4. Reboot Machine
5. When you are restarted, open Command window
6. Type `bash` and press enter
7. Accept license and create Unix user

_As of 04/19/2017 Microsoft managed instructions can be found [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)_


### Install VSCode
Instructions to install VSCode can be found [here](https://code.visualstudio.com/docs/setup/setup-overview)

### VSCode Setup
This section describes how to integrate your Linux terminal into VSCode so that you have an integrated Linux terminal in your IDE. It assumes that you have [Setup your Windows Machine](#user-content-windows-machine-setup).

There are instructions for [Windows 10](#user-content-vscode-for-windows-10) and instructions for [Windows <10](#user-content-vscode-for-windows-<10). You should follow the instructions for your specific OS.

#### VSCode for Windows <10
_For the below you can integrate your terminal at the IDE or the App level:_

* Integration at the IDE level: `Ctrl+,` OR File > Preferences > Settings
* Integration at the App level: `Ctrl+Shift+P` > "Workspace Settings"

1. Open settings for desired integration point
2. Add the following to the file: 	

##### For Cygwin Installation
```json
// Place your settings in this file to overwrite the default settings
{
    "terminal.integrated.shell.windows": "C:\\cygwin\\bin\\bash.exe",
    "terminal.integrated.shellArgs.windows": ["/bin/xhere", "/bin/bash"]    
}

```

##### For Cygwin64 Installation
```json
// Place your settings in this file to overwrite the default settings
{
    "terminal.integrated.shell.windows": "C:\\cygwin64\\bin\\bash.exe",
    "terminal.integrated.shellArgs.windows": ["/bin/xhere", "/bin/bash"]    
}

```

#### VSCode for Windows 10
_For the below you can integrate your terminal at the IDE or the App level:_

* Integration at the IDE level: `Ctrl+,` OR File > Preferences > Settings
* Integration at the App level: `Ctrl+Shift+P` > "Workspace Settings"

1. Open settings for desired integration point
2. Add the following to the file: 	

```json
// Place your settings in this file to overwrite the default settings
{
	"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"   
}

```
#### VSCode Native Unit Test Debugging

1. Go to Debug > Add Configuration...
2. Paste the following snippet into the `launch.json` file

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mocha Tests",
  "args": [
    "-R",
    "spec",
    "--compilers",
    "js:babel-register",
    "--timeout",
    "0",
    "--ui",
    "bdd",
    "${workspaceRoot}/test/ut/**/*.test.js"
  ],
  "protocol": "inspector",
  "program": "${workspaceRoot}/node_modules/mocha/bin/_mocha",
  "internalConsoleOptions": "openOnSessionStart"
}
```

3. On the left side project navigation click the Debug icon _(it is the second up from the bottom in a default installation)_.
4. In the debug pannel that is now open, find and select the "Mocha Tests" configuration in the drop down at the top of the debug pannel.
5. Add any desired breakpoints
6. Press the "Play" button to the left of your "Mocha Tests" configuration selection.


### NVM Setup
NVM is a Node Version Manager. It is highly recommended that you install a Node Version Manager so that you will easily be able to support development across multiple versions of Node. This section describes how to set up NVM.

#### NVM For Windows
NVM does not support Windows, but there is a sister project nvm-windows that offers much of the functionality of nvm on Windows. It is a great project and the recommended nvm for Windows.

The installer and installation instructions can be found [here](https://github.com/coreybutler/nvm-windows)
