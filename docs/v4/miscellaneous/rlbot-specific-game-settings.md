RLBot has the ability to change game settings only when RLBot is active. This can be useful when a different environment is desired for developing or running bots compared to playing the game yourself.

This is achieved by briefly altering *TASystemSettings.ini* as RLBot starts up Rocket League. This feature is currently only available on windows, and for the default *TASystemSettings.ini* location of *%USERPROFILE%\\Documents\\My Games\\Rocket League\\TAGame\\Config*.

To have settings that only apply when using RLBot, a *TASystemSettings.RLBot.ini* file has to be created in the same folder as *TASystemSettings.ini*. This file does **not** need to be a full copy of *TASystemSettings.ini*, but should **only** contain the **differences** with *TASystemSettings.ini* to improve compatibility with Rocket League updates. When RLBot starts up, the changes found in *TASystemSettings.RLBot.ini* will be temporarily applied to *TASystemSettings.ini*.

The remainder of this page contains some example use cases.

## Cap the in game FPS to 120 for RLBot, uncap the FPS during normal play

This is useful to create the most consistent environment for RLBot. See [Tick Rate](/v4/botmaking/tick-rate) for more info.

- The FPS will have to be uncapped during normal play. This is achieved by setting `AllowPerFrameSleep=False` in *TASystemSettings.ini*.
- In the game pause screen, the fps cap has to be set to 120 exactly.
- *TASystemSettings.RLBot.ini* should contain the following configuration to turn the FPS cap back on for RLBot:

```
[SystemSettings]
AllowPerFrameSleep=True
```

## Other Useful Settings For Bot Testing

in \[SystemSettings\]:

- ResX=1280 (Change the x value of your resolution)
- ResY=720 (Change the y value of your resolution)
- Fullscreen=False (False for window mode, True for fullscreen)
- Borderless=True (enable borderless for windowed mode aka Fullscreen=False)
