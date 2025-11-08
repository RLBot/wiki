# Installation errors

### You are using a third-party antivirus

RLBotGUI may open for a second before closing, it may install but fail to work, or produce other weird behavior.
A list of known antivirus-related error messages is listed down below under Miscellaneous Issues

1. Check out our other wiki page: [Antivirus Notes](../antivirus-notes)

### RLBotGUI gets stuck updating/collecting packages during installation

This usually happens because of:

1. slow internet (try giving it 5 minutes)
2. pip (python's package installer) broke

If you've given it time and nothing's happened:

1. Restart your computer and try again
2. If that doesn't work, delete pip's cache folder located in `%localappdata%/pip` and try again

# Problems starting a match

Check the console window for any error messages.

### Encountered a std Exception: listen

Another program is preventing RLBot from talking to Rocket League.

1. Restart your computer

### Encountered a std Exception: Unknown Error

A Bakkesmod plugin that you are using isn't compatible with RLBot.

1. Disable Bakkesmod
2. Restart Rocket League and RLBotGUI

### The game is open but no match started

1. Ensure you started with Rocket League closed. RLBotGUI has to launch it for you
2. Check for the above errors in the console
3. Check your antivirus to ensure it didn't delete RLBot's files

### The game doesn't launch and there's no error in the console

1. Ensure you set RLBotGUI to launch from the correct (Steam/Epic) platform.

### java.lang.RuntimeException: java.nio.charset.MalformedInputException: Input length = 1

This currently happens to people with strange letters/characters in their Steam username.

1. Change your steam username

# Miscellaneous Issues

### Common Antivirus-related error messages

- `FileNotFoundError [WinError 2]: The system cannot find the file specified`
- `Please check that the file exists and your antivirus is not removing it`
- `Windows: OSError: [WinError 1450] Insufficient system resources exist to complete the requested service`
- `Encountered exception:  [WinError 5] Access is denied`
- `[WinError 225] Operation did not complete successfully because the file contains a virus or potentially unwanted software`

For all of the above:

1. Check out our other wiki page for this issue: [Antivirus Notes](../antivirus-notes)

### Cannot proceed because VCRUNTIME140.dll was not found

Your computer may be missing some important files, which you can download from here:

- [https://go.microsoft.com/fwlink/?LinkId=746572](https://go.microsoft.com/fwlink/?LinkId=746572)
- [https://go.microsoft.com/fwlink/?LinkId=746571](https://go.microsoft.com/fwlink/?LinkId=746571)

### Application Error: The application was unable to start correctly (0xc000007b).

1. Restart your computer

### Frame stuttering when using RLBot

1. Restart your computer
2. Close or disable DropBox if it is running

# Bot development Issues

### Python Acting Super Weird

Sometimes on Windows 10, `~\AppData\Local\Microsoft\WindowsApps` will end up on your system path, which supplies a fake python.exe taking you to the Microsoft Store. This causes bizarre things to happen, e.g. nothing at all being printed when you run `python --version`.

You can confirm whether this is happening to you by opening a command prompt and running `where.exe` like this:

```
>where.exe python
C:\Users\tareh\AppData\Local\Microsoft\WindowsApps\python.exe
```

If it shows the Microsoft\\WindowsApps\\python.exe one at the top, you need to fix it. You can try [this](https://superuser.com/questions/1437590/typing-python-on-windows-10-version-1903-command-prompt-opens-microsoft-store)

### java.lang.UnsatisfiedLinkError: A dynamic link library (DLL) initialization routine failed.

1. Run your .bat files as administrator to get past this error.

### Could not locate a suitable bot class in module

This means that your bot's python file was found, but there's something messed up in it.

1. Ensure you saved the file
2. Check to make sure your bot extends the framework's agent class

### Could not find a version that satisfies the requirement PyQt5

Your system might be using python 2 instead of python 3. This can happen even if you successfully installed python 3 and added it to your path. Verify whether this is the case by running `python --version`. If it says python 2, try these:

1. Make sure you've installed python 3 and chosen "Add to PATH" during the installation.
2. Open the windows "environment variables" dialog and make sure that python 3 is listed *before* python 2.

### Scratch: The project file that was selected failed to load.

We're using Scratch 3.0 which is pre-release and a bit buggy. Sometimes the save gets corrupted. Here's how to avoid that, and recover your file if it does get corrupted:

- If you save a vector in a variable then save your project, it will be broken.
- If you somehow insert null values into a list then save your project, it will be broken.

General strategy for recovering the file:

1. Hit F12 in your browser to view the javascript console and read the error that appears when you try to upload the broken file. It will look something like this:

```
gui {"validationError":"Could not parse as a valid SB2 or SB3 project.","sb2Errors":[{"keyword":"required","dataPath":"","schemaPath":"#/required","params":{"missingProperty":"objName"},"message":"should have required property 'objName'"}],"sb3Errors":[{"keyword":"type","dataPath":".targets[0].lists['g*`|xWn5EppiH.9NKPVF'][1][0]","schemaPath":"#/definitions/stringOrNumber/oneOf/0/type","params":{"type":"string"},"message":"should be string"},{"keyword":"type","dataPath":".targets[0].lists['g*`|xWn5EppiH.9NKPVF'][1][0]","schemaPath":"#/definitions/stringOrNumber/oneOf/1/type","params":{"type":"number"},"message":"should be number"},{"keyword":"oneOf","dataPath":".targets[0].lists['g*`|xWn5EppiH.9NKPVF'][1][0]","schemaPath":"#/definitions/stringOrNumber/oneOf","params":{"passingSchemas":null},"message":"should match exactly one schema in oneOf"},{"keyword":"type","dataPath":".targets[0].lists['g*`|xWn5EppiH.9NKPVF'][1][0]","schemaPath":"#/oneOf/1/type","params":{"type":"boolean"},"message":"should be boolean"},{"keyword":"oneOf","dataPath":".targets[0].lists['g*`|xWn5EppiH.9NKPVF'][1][0]","schemaPath":"#/oneOf","params":{"passingSchemas":null},"message":"should match exactly one schema in oneOf"}]}
```

2. Your sb3 is really a zip file, so unzip it and look inside project.json.
3. Search for xWn5EppiH.9NKPVF and see if there's anything funky saved in it, and manually make a fix. For a list, you could delete the list items. For a variable, you could set it to "".
4. Then put the fixed version of project.json back in the zip and try uploading again.

### Import errors:

If you run into an import error that works fine when testing outside of rlbot but crashes when loading your agent, it can most often be solved by simply moving the import statement to your agent's init method. This is helpful for libraries such as tensorflow and rlutilities

# Old issues that are probably irrelevant

### 'pip 'is not recognized as an internal or external command

Something blocked RLBotGUI from installing Python on the first try, and you have to reinstall by following these steps:

1. Press `Windows + R`, and type in `%localappdata%\RLBotGUIX` and then press Enter
2. Delete the folders `Python37` and `venv`
3. Relaunch RLBotGUI.exe

### The term 'Expand-Archive' is not recognized as the name of a cmdlet

You are using an older version of Windows and must manually perform some of the installation with the following steps:

1. Download our copy of Python from [here](https://github.com/RLBot/RLBotGUI/blob/master/alternative-install/python-3.7.9-custom-amd64.zip)
2. Press `Windows + R`, type in `%localappdata%\RLBotGUIX` and then press Enter
3. Create a new folder called `Python37` and open the folder
4. Find the zip file of Python you downloaded and double click it
5. Drag the contents from the zip file into the folder Python37
