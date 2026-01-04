---
search:
  boost: 0.5
---

## Windows

RLBot was originally made for Windows, so Windows has the best support. Users are encouraged to watch [this video](https://www.youtube.com/watch?v=oXkbizklI2U) to get started quickly with RLBotGUI!

## Linux

RLBot v4 only runs on Linux with the old native Linux version of Rocket League. This version is only available on Steam. There is a (probably outdated) install script [here](https://github.com/RLBot/RLBotGUI/tree/master/linux-install).

If you only have the Epic version of the game or you want to run the Proton version, you're unfortunately out of luck. This is due to [libRLBotInterface.so](https://github.com/RLBot/RLBot/blob/master/src/main/python/rlbot/dll/libRLBotInterface.so) being broken on Linux.

## Mac

Mac support is currently unknown and may not work. If you want to try, see the instructions below.

### Instructions

1. Make sure you have Python 3.11 and pip
2. Run these commands

```sh
python3 -m pip install --user --upgrade pip
python3 -m pip install --user gevent eel
python3 -m pip install --user --upgrade rlbot rlbot_gui
python3 -c "from rlbot_gui import gui; gui.start()"
```
