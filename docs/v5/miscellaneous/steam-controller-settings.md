# Steam Controller Settings

Steam offers various controller settings such as custom deadzones.
By registering Rocket League as a non-Steam game on Steam these features become available for Epic Games installations of Rocket League too.
Unfortunately, RLBot cannot launch non-Steam games automatically.
The workaround is to launch Rocket League in RLBot mode manually.
By using [Slipstream](https://github.com/jun-eau/Slipstream) you will be logged in to your Epic account so you get your items/settings.

Steps:

- Download and add [Slipstream](https://github.com/jun-eau/Slipstream) as a non-Steam game on Steam.
- Right click, open Properties, and add the launch options: `-rlbot RLBot_ControllerURL=127.0.0.1:23233 RLBot_PacketSendRate=240 -nomovie -noeac`
- Launch it to open Rocket League in NoEAC+RLBot mode.
- Use RLBot as normal.

You can have two instances of Slipstream on Steam so you don't have to edit the launch options each time you want to use RLBot.