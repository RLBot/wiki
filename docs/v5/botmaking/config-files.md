# RLBot config files

## Bot & script config files

There's a new naming convention for the toml files:

- Config files that define bots MUST either be named `bot.toml` or end in `.bot.toml`. Here are examples of valid names:
  - `bot.toml`
  - `Necto.bot.toml`
  - `atba.bot.toml`
- Config files that define scripts MUST either be named `script.toml` or end in `.script.toml`. Here are examples of valid names:
  - `script.toml`
  - `tron.script.toml`
  - `SCB.script.toml`

It should also be noted that whatever prefixes the `.bot.toml`/`.script.toml` file name will not be used by anything in RLBot.

Example of a `bot.toml` file that runs in a virtual environment:

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

Example of a `script.toml` file that runs in the global Python environment:

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

- `[settings]` - Used by both RLBot & the GUI
  - `agent_id` - The static, unique id that is associated with this bot.
    - Preferred format is `"author/bot-name"`
  - `name` - The name of the bot/script.
  - `loadout_file` - The path to the loadout file for the bot.
  - `run_command` - The command to run the bot/script on Windows.
  - `run_command_linux` - The command to run the bot/script on Linux.
  - `logo_file` - The path to the logo file for the bot/script.
- `[details]` - Used only by the GUI
  - `description` - A description of the bot/script.
  - `fun_fact` - A fun fact about the bot/script.
  - `source_link` - A link to the source code of the bot/script.
  - `developer` - The developer(s) of the bot/script.
  - `language` - The language the bot/script is written in.
  - `tags` - A list of tags that describe the bot/script. Possible tags:
    - `1v1`
    - `teamplay`
    - `goalie` - Only add this tag if your bot only plays as a goalie; this directly contrasts with the teamplay tag!
    - `hoops`
    - `dropshot`
    - `snow-day`
    - `spike-rush`
    - `heatseaker`
    - `memebot`

## Loadout config files

There's fewer differences between the `.toml` and `.cfg` files for loadouts - all of they keys have stayed the same, only the headers are differently named.

- `[Bot Loadout]` -> `[blue_loadout]`
- `[Bot Loadout Orange]` -> `[orange_loadout]`
- `[Bot Paint Blue]` -> `[blue_loadout.paint]`
- `[Bot Paint Orange]` -> `[orange_loadout.paint]`

Example of a `loadout.toml` file:

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
