## Windows

RLBot was originally made for Windows, so Windows has the best support. Users are encouraged to watch [this video](https://www.youtube.com/watch?v=oXkbizklI2U) to get started quickly with RLBotGUI!

## Instructions for Debian-based Linux

RLBotGUI has it's own installer/launcher for Debian-based Linux. Download [RLBotGUI.sh](https://raw.githubusercontent.com/RLBot/RLBotGUI/master/linux-install/RLBotGUI.sh) to get it.

[View this page for more information](https://github.com/RLBot/RLBotGUI/tree/master/linux-install). This includes information on how to run shell scripts, and what commands need sudo in order to run.

## Mac and Non-Debian based Linux

RLBot gained basic Linux support on 2019-09-03, and basic Mac support on 2019-12-07.

RLBotGUI (not the RLBot Framework!) gained full support for Debian-based Linux distros on 2020-06-21.

2020-9-23 - Running Rocket League Mac and Linux is not possible if you have Rocket League on the Epic Games Launcher. Therefore in order to use Rocket League (and RLBot), you must have bought Rocket League before this date on Steam.

- Mac Big Sur is currently incompatible as of 2021-07-07.
- Tested in Ubuntu 20.04 and Mac High Sierra with basic python bots.
- Many bots and languages don't work yet.
- Our Mac and Linux binaries may not be updated as frequently as the Windows ones.

### Instructions

1. Make sure you have Python 3.7 or higher (`sudo apt install python3`).
2. Make sure you have pip (`sudo apt install python3-pip`).
3. Make yourself a shell script with:

```
python3 -m pip install --user --upgrade pip
python3 -m pip install --user gevent eel
python3 -m pip install --user --upgrade rlbot rlbot_gui
python3 -c "from rlbot_gui import gui; gui.start()"
```

Run it and expect the GUI to open. Check the windows video to see what to expect. Make sure Rocket League is **closed** before starting your first match.

### Troubleshooting

- No module named setuptools: try https://stackoverflow.com/questions/14426491/python-3-importerror-no-module-named-setuptools
- Header Python.h does not exist: try installing the package python-dev
