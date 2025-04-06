# Configuration Files

RLBot uses the standard `.toml` format for all configuration files. To see how TOML works, check out the [TOML spec](https://toml.io/en/).

The framework uses four types of config files, commonly known as:

- `bot.toml` [GOTO](#bot-script-config-files)
- `script.toml` [GOTO](#bot-script-config-files)
- `loadout.toml` [GOTO](#loadout-config-files)
- `match.toml` [GOTO](#match-config-files)

## Bot & Script Config Files

A bot/script config file defines a bot/script and its attributes, closely resembling the `PlayerConfiguration`/`ScriptConfiguration` from the [flatbuffer schema](https://github.com/RLBot/flatbuffers-schema/blob/main/matchconfig.fbs).
The content of config files for bots and scripts are very similar, so the file extension indicates whether it is a bot or script:

- **Bots:** Named `bot.toml` or ends with `.bot.toml`. Here are examples of valid names: `bot.toml`, `Necto.bot.toml`, `atba.bot.toml`
- **Scripts:** Named `script.toml` or ends with `.script.toml`. Here are examples of valid names: `script.toml`, `tron.script.toml`, `SCB.script.toml`

It should also be noted that whatever prefixes the `.bot.toml`/`.script.toml` file name will not be used by anything in RLBot.

**The sections and fields:**

- `[settings]` - Used by both RLBot and the GUI
    - `agent_id` - The static, unique id that is associated with this bot. Preferred format is `"author/bot-name"`.
    - `name` - The name of the bot/script.
    - `loadout_file` - The path to the [loadout file](#loadout-config-files) for the bot. *Ignored for scripts.*
    - `root_dir` - A path to the root directory of the bot, e.g. `"./bin/"`. The `run_command` will be run from the root directory.
    - `run_command` - The command to run the bot/script on Windows. If empty, the bot must be started manually (This may be handy during development if a default `agent_id` is coded into the bot).
    - `run_command_linux` - The command to run the bot/script on Linux.
    - `hivemind` - Boolean. Whether all bot team mates with this agent id is controlled by the same process. See [Hiveminds](v5/botmaking/hiveminds). *Ignored for scripts.*
    - `logo_file` - The path to the logo file for the bot/script.
- `[details]` - Used only by the GUI
    - `description` - A description of the bot/script.
    - `fun_fact` - A fun fact about the bot/script.
    - `source_link` - A link to the source code of the bot/script.
    - `developer` - The developer(s) of the bot/script.
    - `language` - The language the bot/script is written in.
    - `tags` - A list of tags that describe the bot/script. These will affect which categories in the GUI your bot/script appear in. Possible tags:
        - `1v1` - The bot plays traditional 1v1 soccer.
        - `teamplay` - The bot considers team mates and rotates.
        - `goalie` - Only add this tag if your bot *only* plays as a goalie. Incompatible with teamplay tag!
        - `hoops` - The bot understands the Hoops game mode.
        - `dropshot` - The bot understands the Dropshot game mode.
        - `snow-day` - The bot understands the Snow Day game mode.
        - `spike-rush` - The bot understands the Spike-Rush game mode.
        - `heatseeker` - The bot understands the Heatseeker game mode.
        - `memebot` - The bot has an untraditional play style.

!!! note "Psyonix Bot Presets"
    If a Psyonix bot is not given a name, it will be a random [standard Psyonix bot](https://rocketleague.fandom.com/wiki/Bot). If a Psyonix bot's name matches a standard Psyonix bot, it will use the standard loadout of that bot unless a loadout file is specified.

??? example "Example `bot.toml` that runs a Python bot in a virtual environment"

    ```toml
    [settings]
    agent_id = "rolv-soren/necto"
    name = "Necto"
    loadout_file = "loadout.toml"
    run_command = ".\\venv\\Scripts\\python bot.py"
    run_command_linux = "./venv/bin/python bot.py"
    logo_file = "necto_logo.png"

    [details]
    description = "Necto is the official RLGym community bot, trained using PPO with workers run by people all around the world."
    fun_fact = "Necto uses an attention mechanism, commonly used for text understanding, to support any number of players"
    source_link = "https://github.com/Rolv-Arild/Necto"
    developer = "Rolv, Soren, and several contributors"
    language = "rlgym"
    tags = ["1v1", "teamplay"]
    ```

??? example "Example `script.toml` that runs in the global Python environment"

    ```toml
    [settings]
    agent_id = "rlbot-community/script-example"
    name = "Script Example"
    run_command = "python render.py"
    run_command_linux = "python3 render.py"
    logo_file = "logo.png"

    [details]
    description = "Script example"
    fun_fact = "This is just an example TOML!"
    source_link = ""
    developer = "The RLBot community"
    language = "python"
    tags = []
    ```

## Loadout Config Files

Loadout config files, e.g. `loadout.toml`, define a bot's default loadout. They are tedious to make by hand, so we recommend using the [GUI's loadout maker](/botmaking/bot-customization). The item ids can be found [here](https://github.com/RLBot/RLBotGUI/blob/master/rlbot_gui/gui/csv/items.csv). Note that it is also possible to set your bots loadout through code. See [Set Loadout](botmaking/manipulating-game-state).


??? example "Example `loadout.toml`"

    ```toml
    [blue_loadout]
    # Primary Color selection
    team_color_id = 29
    # Secondary Color selection
    custom_color_id = 0
    # Car type (Octane, Merc, etc)
    car_id = 23
    # Type of decal
    decal_id = 6083
    # Wheel selection
    wheels_id = 1580
    # Boost selection
    boost_id = 35
    # Antenna Selection
    antenna_id = 0
    # Hat Selection
    hat_id = 0
    # Paint Type (for first color)
    paint_finish_id = 0
    # Paint Type (for secondary color)
    custom_finish_id = 0
    # Engine Audio Selection
    engine_audio_id = 6919
    # Car trail Selection
    trails_id = 3220
    # Goal Explosion Selection
    goal_explosion_id = 4118

    [orange_loadout]
    team_color_id = 69
    custom_color_id = 0
    car_id = 23
    decal_id = 6083
    wheels_id = 1580
    boost_id = 35
    antenna_id = 0
    hat_id = 0
    paint_finish_id = 1681
    custom_finish_id = 1681
    engine_audio_id = 5635
    trails_id = 3220
    goal_explosion_id = 4118

    [blue_loadout.paint]
    car_paint_id = 12
    decal_paint_id = 12
    wheels_paint_id = 12
    boost_paint_id = 12
    antenna_paint_id = 0
    hat_paint_id = 0
    trails_paint_id = 12
    goal_explosion_paint_id = 12

    [orange_loadout.paint]
    car_paint_id = 12
    decal_paint_id = 12
    wheels_paint_id = 12
    boost_paint_id = 12
    antenna_paint_id = 0
    hat_paint_id = 0
    trails_paint_id = 12
    goal_explosion_paint_id = 12
    ```

## Match Config Files

A match config file, e.g. `match.toml`, define a match and its settings, closely resembling `MatchConfiguration ` from the [flatbuffer schema](https://github.com/RLBot/flatbuffers-schema/blob/main/matchconfig.fbs).

**Sections and fields:**

- `[rlbot]`
    - `launcher` - Indicates how Rocket League should be launched if it is not already running. Either `"Steam"` (default), `"Epic"`, `"Custom"` (see `launcher_arg`), or `NoLaunch`.
    - `launcher_arg` - Additional configuration to the launching method:
        - `"legendary" (Custom)` - Start Rocket League using [Legendary](https://github.com/derrod/legendary)
    - `auto_start_bots` - Boolean (default: true). Whether bots and scripts should be started using their run command. It is also possible to disable auto-starting for individual bots and scripts. See `[cars]::auto_start` and `[scripts]::auto_start`.
    - `wait_for_bots` - Boolean (default: true). Whether RLBot should wait for bots and scripts to be connected and ready before starting the match.
- `[match]`
    - `game_mode` - The game mode. Either `"Soccer"` (default), `"Hoops"`, `"Dropshot"`, `"Hockey"`, `"Rumble"`, `"Heatseeker"`, `"Gridiron"`, or `"Knockout"`. This affects ball prediction and a few of the game rules although many game modes can also be recreated solely from mutators. See what mutators and game mode combinations make up the official modes [here](https://github.com/RLBot/python-interface/tree/master/tests/gamemodes).
    - `game_map_upk` - The map upk file to load, e.g. `"UtopiaStadium_P"`. On Steam version of Rocket League this can be used to load custom map files, but on Epic version it only works on the Psyonix maps. Available maps can be found [here](https://github.com/RLBot/python-interface/blob/master/rlbot/utils/maps.py).
    - `cars` - A list of players in the match. See the car section below for fields of cars.
    - `scripts` - A list of scripts in the match. See the script section below for fields of scripts.
    - `skip_replays` - Boolean (default: false). Whether to skip goal replays.
    - `start_without_countdown` - Boolean (default: false). Whether to start without kickoff countdown.
    - `existing_match_behavior` - How to handle any ongoing match (if no match is running, a new one is started). Either:
        - `"Restart"` (default): Always start a new match.
        - `"RestartIfDifferent"`: Starts a new match unless the match configuration is completely identical to the previous one.
        - `"ContinueAndSpawn"`: Let the match continue and spawn/despawn bots to match the new match configuration. Specifically:
            - For each player index, if the team or agent id is different in the new match config, that player's car is despawned. Other cars are completely unaffected, and new cars are spawned for the new players.
            - All bot and script processes are restarted, even if they control a car that is not despawned. As a consequence, the restarted process receives the updated match config.
            - The local human player is also spawned or set to spectator based on the new match config. Additional humans are not affected due to limitations.
            - Mutators are not affected, but debug rendering and state setting options are updated.
    - `enable_rendering` - Boolean (default: false). Whether debug rendering is displayed.
    - `enable_state_setting` - Boolean (default: true). Whether bots and scripts are allowed to manipulate the game state, e.g. teleporting cars and ball.
    - `auto_save_replay` - Boolean (default false). Whether the match replay should be saved even if match is stopped early (useful when match length is unlimited).
    - `freeplay` - Boolean(default false). Whether or not to use freeplay instead of an exhibition match. This allows the players to use training keybinds, Bakkesmod plugins, and other features that are only allowed in free play.
- `[mutators]`
    - `match_length` - Duration of the match. Either `"FiveMinutes"` (default), `"TenMinutes"`, `"TwentyMinutes"`, or `"Unlimited"`.
    - `max_score` - Max score of match. If this score is reached, the team immediately wins. Either `"Unlimited"` (default), `"OneGoal"`, `"ThreeGoals"`, `"FiveGoals"`, `"SevenGoals"`, `"TenGoals"`, `"TwentyGoals"`, `"ThirtyGoals"`, `"FortyGoals"`, `"FiftyGoals"`, `"SixtyGoals"`, `"SeventyGoals"`, `"EightyGoals"`, `"NinetyGoals"`, or `"HundredGoals"`.
    - `multi_ball` - The number of balls. Either `"One"` (default), `"Two"`, `"Four"`, or `"Six"`.
    - `overtime` - The overtime rules and tiebreaker. Either `"Unlimited"` (default), `"FiveMaxFirstScore"`, or `"FiveMaxRandomTeam"`.
    - `series_length` - The series length. Either `"Unlimited"` (default), `"ThreeGames"`, `"FiveGames"`, or `"SevenGames"`.
    - `game_speed` - A game speed multiplier. Either `"Default"`, `"SloMo"`, or `"TimeWarp"`.
    - `ball_max_speed` - Ball max speed. Either `"Default"`, `"Slow"`, `"Fast"`, or `"SuperFast"`.
    - `ball_type` - Ball type and shape. Either `"Default"`, `"Cube"`, `"Puck"`, `"Basketball"`, `"Beachball"`, `"Anniversary"`, `"Haunted"`, `"Ekin"`, `"SpookyCube"`, `"Egg"`, `"PlayerSeeking"`, `"Dropshot"`, or `"ScoreAbsorb"`.
    - `ball_weight` - Ball weight and how much is curves. Either `"Default"`, `"Light"`, `"Heavy"`, `"SuperLight"`, `"CurveBall"`, `"BeachBallCurve"`, or `"MagnusFutBall"`.
    - `ball_size` - Ball size. Either `"Default"`, `"Small"`, `"Medium"`, `"Large"`, or `"Gigantic"`.
    - `ball_bounciness` - Ball bounciness. Either `"Default"`, `"Low"`, `"High"`, `"SuperHigh"`, or `"LowishBounciness"`.
    - `boost_amount` - Boost amount/recharge. Either `"NormalBoost"`, `"UnlimitedBoost"`, `"SlowRecharge"`, `"RapidRecharge"`, or `"NoBoost"`.
    - `rumble` - Rumble item rules. Either `"Off"` (default), `"DefaultRumble"`, `"Slow"`, `"Civilized"`, `"DestructionDerby"`, `"SpringLoaded"`, `"SpikesOnly"`, `"SpikeRush"`, `"HauntedBallBeam"`, `"Tactical"`, `"BatmanRumble"`, `"GrapplingOnly"`, or `"HaymakerOnly"`.
    - `boost_strength` - Boost strength multiplier. Either `"One"` (default), `"OneAndAHalf"`, `"Two"`, `"Five"`, or `"Ten"`.
    - `gravity` - Strength of gravity. Either `"Default"`, `"Low"`, `"High"`, `"SuperHigh"`, or `"Reverse"`.
    - `demolish` - Demolition conditions. Either `"Default"`, `"Disabled"`, `"FriendlyFire"`, `"OnContact"`, `"OnContactFF"`, `"OnBallContact"`, or `"OnBallContactFF"`.
    - `respawn_time` - Demolition respawn time. Either `"ThreeSeconds"` (default), `"TwoSeconds"`, `"OneSecond"`, or `"DisableGoalReset"`.
    - `max_time` - Max real-time duration of match including kickoff, replays, and more. If the score is tied upon time-out, the number of shots determine the winner. Either `"Unlimited"` (default) or `"ElevenMinutes"`.
    - `game_event` - Additional game behavior for custom modes. Either `"Default"`, `"Haunted"`, or `"Rugby"`.
    - `audio` - Additional audio options for custom modes. Either `"Default"` or `"Haunted"`.
    - `ball_gravity` - Ball gravity. Either `"Default"`, `"Low"`, `"High"`, or `"SuperHigh"`.
    - `territory` - Disallow players from touching the opponents side if the field. Usually combined with `the stale_ball` mutator. Either Either `"Off"` (default) or `"Territory"`.
    - `stale_ball` - Disallow the ball to stay on one half of the field by teleporting it to the other half after the given amount of time. Usually combined with the `territory` mutator. Either `"Unlimited"` (default) or `"ThirtySeconds"`.
    - `jump` - How many times you can jump and dodge/double jump. Either `"Default"`, `"Grounded"`, `"Two"`, `"Three"`, `"Four"`, `"Unlimited"`, or `"NoJumps"`.
    - `dodge_timer` - How long you can wait before dodging or double jumping. Either `"OnePointTwentyFiveSeconds"` (default), `"TwoSeconds"`, `"ThreeSeconds"`, or `"Unlimited"`.
    - `possession_score` - Modifies how often possession gives points. Specifically, the team that touched the ball most recently gains a point every *X* seconds until the other team touches the ball. Either `"Off"` (default), `"OneSecond"`, `"TwoSeconds"`, or `"ThreeSeconds"`.
    - `demolish_score` - Modifies how many points demolitions score. Either `"Zero"` (default), `"One"`, `"Two"`, or `"Three"`.
    - `normal_goal_score` - Modifies how many points a normal goal score. Either `"One"` (default), `"Zero"`, `"Two"`, `"Three"`, `"Five"`, or `"Ten"`
    - `aerial_goal_score` - Modifies how many points aerial goals score. Either `"One"` (default), `"Zero"`, `"Two"`, `"Three"`, `"Five"`, or `"Ten"`.
    - `assist_goal_score` - Modifies how many points assists score. Either `"Zero"` (default), `"One"`, `"Two"`, or `"Three"`.
    - `input_restriction` - Restricts player inputs. Either `"Default"` or `"Backwards"`.

**Car fields:**

- `team` - The team of the player. Either `"Blue"`/`0` for blue or `"Orange"`/`1` for orange. Default 0.
- `type` - Determines what controls the car. Either `"RLBot"` (default), `"Human"`, or `"Psyonix"` (see `skill`), 
- `skill` - Determines the skill level of a Psyonix bot. Either `"Beginner"`, `"Rookie"`, `"Pro"`, and `"Allstar"`.
- `config_file` - A path to a [`bot.toml` config file](#bot-script-config-files). Unused if `type` is not `"RLBot"` or `"Psyonix"`. For Psyonix bots, the config file determines name and loadout.
- `name` - Overrides the name in `config`.
- `loadout_file` - Overrides the loadout in `config`.
- `auto_start` - Boolean (default true). Whether the bot should be started using its run command. If false, the run command will be considered empty. See also `auto_start_bots` to disable auto-start for all bots and scripts.

**Script fields:**

- `config_file` - A path to a [`script.toml` config file](#bot-script-config-files).
- `auto_start` - Boolean (default true). Whether the script should be started using its run command. If false, the running will be considered empty. See also `auto_start_bots` to disable auto-start for all bots and scripts.

??? example "Example `match.toml` with default values + some cars and scripts"

    ```toml
    [rlbot]
    launcher = "Steam"
    auto_start_bots = true
    wait_for_bots = true

    [match]
    game_mode = "Soccer"
    game_map_upk = "Stadium_P"
    skip_replays = false
    start_without_countdown = false
    existing_match_behavior = "Restart"
    enable_rendering = false
    enable_state_setting = true
    auto_save_replay = false
    freeplay = false

    [[cars]]
    config_file = "atba/atba.bot.toml"
    # The following unnessary since they are default values
    team = 0
    type = "RLBot"
    auto_start = true

    [[cars]]
    team = 0
    config_file = "necto/bot.toml"

    [[cars]]
    team = 1
    type = "Psyonix"
    skill = "Pro"

    [[cars]]
    team = 1
    type = "Human"

    [[scripts]]
    config = "zero-g.script.toml"

    [mutators]
    # All mutators with default values
    match_length = "FiveMinutes"
    max_score = "Unlimited"
    multi_ball = "One"
    overtime = "Unlimited"
    game_speed = "Default"
    ball_max_speed = "Default"
    ball_type = "Default"
    ball_weight = "Default"
    ball_size = "Default"
    ball_bounciness = "Default"
    boost_amount = "NormalBoost"
    rumble = "Off"
    boost_strength = "One"
    gravity = "Default"
    demolish = "Default"
    respawn_time = "ThreeSeconds"
    max_time = "Unlimited"
    game_event = "Default"
    audio = "Default"
    ball_gravity = "Default";
    territory = "Off";
    stale_ball = "Unlimited";
    jump = "Default";
    dodge_timer = "OnePointTwentyFiveSeconds";
    possession_score = "Off";
    demolish_score = "Zero";
    normal_goal_score = "One";
    aerial_goal_score = "One";
    assist_goal_score = "Zero";
    input_restriction = "Default";
    ```
