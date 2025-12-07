
There are three ways to play with custom bots together with other players. They are:

1. [Rocket Host](#option-1-rocket-host)
1. [LAN Private Matches](#option-2-lan-private-match)
1. [LAN Splitscreen](#option-3-lan-splitscreen)

Note: It is not possible to play with console players.

## Option 1 - Rocket Host

Rocket Host is a match hosting service eliminating the need for Hamachi and similar. As of 2024-12-12 it supports RLBot allowing you to add custom bots to the matches.

Please help cover the server expenses by supporting [JetFox on Patreon](https://www.patreon.com/c/WcW).

### Requirements

- [BakkesMod](http://www.bakkesmod.com/)
- [Rocket Host](https://bakkesplugins.com/plugins/216) installed in Bakkesmod

### Instructions for Host

1. Start Rocket League and make sure Bakkesmod is running.
1. Open the Rocket Host plugin window (F8) and navigate to the RLBot tab.
1. Pick a nearby server.
1. Add some bots (note: some combinations are unsupported by request of bot developers).
1. Request a server. This will give you an IP/port that you and your friends can join.
1. Useful quick chat commands:
    - Use `!claim` to ensure only you can use commands.
    - Use `!lock` to make the server appear as full (preventing random people from joining).
    - Use `!kick <playername>` to kick a player (part of name works too).


### Instructions for Others

1. Start Rocket League and make sure Bakkesmod is running.
1. Wait for host to send an IP/port.
1. Join the server in the Rocket Host plugin window (F8).

## Option 2 - LAN Private Match

### Requirements

- [Hamachi](https://www.vpn.net/)
- [BakkesMod](http://www.bakkesmod.com/)
- [Rocket Plugin](https://bakkesplugins.com/plugins/view/26) installed in Bakkesmod
- [RLBotGUI](http://rlbot.org/) (only needed for the host)

Quick Note: If the host can forward port 7777 / configure their firewall, then neither the host nor the clients need to use Hamachi. Then the host only has to provide their IP address to the clients who enter it into Rocket Plugin to connect. This allows more connections than the free version of Hamachi.

### Instructions for Host

Only the host can load bots into the game!

1. Create a network in Hamachi and note the network ID (tutorials can be found online).
2. Make sure that BakkesMod is running.
3. Start Rocket League with RLBotGUI (start a match, then quit the match to the menu).
4. Use the Rocket Plugin to host a LAN match
    - Press the "Home" key to open the Rocket Plugin menu.
    - Setting a large team size helps, otherwise bots may de-spawn after demos.
    - Click Host after setting the game options (and password if desired).
5. Tell your friends to join the match via Hamachi + Rocket Plugin (see below for details).
    - If they get a message saying that they could not connect to the host, make sure that you're using the same password for the match as your password for private matches or delete your private match password. See [this post](https://www.reddit.com/r/bakkesmod/comments/iuyqyc/rocket_plugin_not_working_with_multiplayer/g67kiti/) for details.
6. In RLBotGUI, set Extra -> Existing Match Behaviour -> Continue And Spawn.
7. Drag desired bots onto teams in RLBotGUI.
    - Don't worry about adding Human players in RLBotGUI. Human players can simply join ingame.
8. Click Start Match in RLBotGUI. Expect the bots to join the teams.

The setup is basically the same as playing custom maps. If the instructions above are not clear enough, see [this video tutorial by Lethamyr](https://youtu.be/vfIIa2cUZSE).

### Instructions for Clients

Use the standard procedure for joining a LAN match via Rocket Plugin. You can find more detailed tutorials elsewhere, here's a short summary:

1. Open Hamachi and join the host's network via the network ID they tell you.
2. Right-click on the host and click "Copy IPv4 Address".
3. Open Rocket League
4. Open the Rocket Plugin menu with the "Home" key.
5. Under the Multiplayer tab in the "Join a local game" section, paste the host's address into the IP Address field and click Join.
    - The port can be left at its default of 7777.

## Option 3 - LAN Splitscreen

### Requirements

- [Parsec](https://parsecgaming.com/)

### Instructions for Host

Only the host can loads bots into the game!

1. Start Rocket League with RLBotGUI (launch a match, then quit the match to the lobby).
2. Create a Parsec room and have your friends join
3. Start new match with bots and players
4. Have friends join appropriate team

### Instructions for Others

1. Join Parsec room
2. Press Start to join local Rocket League lobby
3. Join appropriate team
