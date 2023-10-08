# Bot Config File

## Sections

### Locations
This section is poorly named at this point, but oh well. It includes:

- looks_config: The location of your bot's appearance config, relative to the cfg file.
- python_file: The location of your bot's python file, relative to the cfg file.
- logo_file: Optional. The location of an image that will represent your bot in RLBotGUI, tournament overlays, etc.
- name: The name of your bot in-game.
- supports_early_start: Optional. Indicates whether your bot can handle the early start system. Read below for details.
- requirements_file: Optional. The location of a requirements.txt for your bot. If specified, RLBotGUI will use it to warn people if there's a missing dependency, and they'll have the option to install your requirements.txt file.
- use_virtual_environment: Optional, defaults to False. Installs your bot's requirements from the requirements.txt file into a python venv so they're isolated from potentially conflicting requirements from other bots. See [pr #535](https://github.com/RLBot/RLBot/pull/536) 
- loadout_generator: Optional. Location of a script that can influence your bot's appearance on startup for randomization / index-dependent changes. See [loadout generator](/bot-customization#loadout-generator)
- requires_tkinter: Optional. Set it to true if your bot needs tkinter, and if the user is running a GUI which lacks tkinter, they will be warned.

### Bot Parameters
This is mainly for bot makers to add arbitrary config values useful to them. Generally the framework doesn't care what you put here, except in the case of Scratch bots.

### Details
This section is for metadata about your bot which can be displayed to people in RLBotGUI, tournament overlays, etc. Common details included:

- developer: The name of the developer(s) of the bot.
- description: Textual description of the bot, its playstyle, how to use it, etc.
- fun_fact: A short fun fact about the bot. Sometimes displayed on stream during tournaments.
- github: A github link
- language: Which language/technology the bots was made with
- tags: A comma separated list of tags. Primarily used by the RLBotGUI to put the bot in the correct tabs. Common tags include: 1v1, teamplay, goalie, hoops, dropshot, snow-day, rumble, spike-rush, heatseeker, memebot

## Early Start System
If a bot adds `supports_early_start = True` then it will be started up before the Rocket League match loads, which gives it lots of extra time. To live happily in this situation the bot MUST be able to deal with weird game tick packets, e.g.:

- Could have unexpected bots / unexpected teams
- The bot's index might be higher than the packet's num_cars
- The packet may transition suddenly to a different set of cars, different game type, etc


# Match Config File (rlbot.cfg)
This configuration controls which bots are in the game, what arena to play on, what mutators to use, etc.
[Example config](https://github.com/RLBot/RLBot/blob/master/rlbot.cfg)

## Sections
- RLBot Configuration:  Contains miscellaneous overall configurations
- Team Configuration:  Configurations at the team level
- Match Configuration:  Configurations for a specific match
- Mutator Configuration:  Mutators for a specific match
- Participant Configuration:  What participants are playing
- Scripts: What scripts should be ran

### RLBot Configuration
- extensions_path:  A path to the extension file used later on for extra game controlling needs
- launcher_preference: 'steam' or 'epic'. For people who have the game on both, determines which launcher is preferred for opening Rocket League.

### Team Configuration
NOTE:  None of these take an effect currently

- Team Blue Color:  Changes Blue team color, use 0 to use default color
- Team Blue Name: Changes the Team name to use instead of 'Blue'
- Team Orange Color: Changes Blue team color, use 0 to use default color
- Team Orange Name: Changes the Team name to use instead of 'Orange'

### Match Configuration
- num_participants: The total number of cars that will be spawned into the match.
- game_mode: 'Soccer', 'Hoops', 'Dropshot', 'Hockey', 'Rumble'
- game_map, e.g. "Mannfield". [All possible values](https://github.com/RLBot/RLBot/blob/master/src/main/python/rlbot/parsing/match_settings_config_parser.py#L40-L78)
- skip_replays: If True, replays are automatically skipped after a goal. However, also prevents match replays from being saved.
- start_without_countdown: If True, skips kickoff countdown
- existing_match_behavior: 'Restart If Different', 'Restart', 'Continue And Spawn'
- enable_lockstep: If True, the framework will wait for outputs from all bots before advancing to the next frame.
- enable_rendering: If True, bots' debug rendering is turned on at the start of a match (toggle it with page up/down).
- enable_state_setting: If True, bots are allowed to manipulate the game state (useful during development).
- auto_save_replay: If True, the match replay is automatically saved.

### Mutator Configuration
All of these default to the "normal" value (the first in the list) if you don't specify them.

- Match Length: Changes the length of the match. '5 Minutes', '10 Minutes', '20 Minutes', 'Unlimited'
- Max Score: Changes the number of goals needed to win. 'Unlimited', '1 Goal', '3 Goals', '5 Goals'
- Game Speed: 'Default', 'Slo-Mo', 'Time Warp'
- Overtime: 'Unlimited', '+5 Max, First Score', '+5 Max, Random Team'
- Ball Max Speed: 'Default', 'Slow', 'Fast', 'Super Fast'
- Ball Type: 'Default', 'Cube', 'Puck' or 'Basketball'
- Ball Weight: 'Default', 'Super Light', 'Light' or 'Heavy'
- Ball Size: 'Default', 'Small', 'Large', 'Gigantic'
- Ball Bounciness: 'Default', 'Low', 'High', 'Super High'
- Boost Amount: 'Default', 'Unlimited', 'Recharge (Slow)', 'Recharge (Fast)' or 'No Boost'
- Rumble: 'None', 'Default', 'Slow', 'Civilized', 'Destruction Derby', 'Spring Loaded', 'Spikes Only' or 'Spike Rush'
- Boost Strength: '1x', '1.5x', '2x', '10x'
- Gravity: 'Default', 'Low', 'High' or 'Super High'
- Demolish: 'Default', 'Disabled', 'Friendly Fire', 'On Contact' or 'On Contact (FF)'
- Respawn Time: '3 Seconds', '2 Seconds', '1 Second', 'Disable Goal Reset'

### Participant Configuration
- participant_config_NUMBER:  The path to a participant configuration
- participant_team_NUMBER: what team the bot is on
- participant_type_NUMBER: the type of the bot Accepted values are "human", "rlbot", "psyonix", "party_member_bot", and "controller_passthrough", You can have up to 4 local humans and they must be activated in game or it will crash. If no player is specified you will be spawned in as spectator!
  - human - not controlled by the framework (but must appear before bot entries)
  - rlbot - controlled by the framework
  - psyonix - default bots (skill level can be changed with participant_bot_skill
  - party_member_bot - controlled by an rlbot but the game detects it as a human
  - controller_passthrough - controlled by a human but runs through the framework
- participant_bot_skill_NUMBER: 0.0 is Rookie, 0.5 is pro, 1.0 is all-star
- participant_loadout_config_NUMBER: the path to the loadout.   This overrides the agent config if not None

Example contents:
```
participant_config_0 = my_folder/my_bot.cfg
participant_config_1 = my_folder/other_bot.cfg

participant_team_0 = 0
participant_team_1 = 1

participant_type_0 = rlbot
participant_type_1 = rlbot

participant_bot_skill_0 = 1.0
participant_bot_skill_1 = 1.0
```

### Scripts
This field is optional, by default no scripts will be ran.

- script_config_NUMBER: The path to the script configuration

Example contents:
```
script_config_0 = src/test/python/agents/script/sample_script.cfg
script_config_1 = src/test/python/agents/script/sample_script.cfg
```
