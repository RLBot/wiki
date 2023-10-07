Sometimes it can be useful to have access to game values, debug rendering or state setting while not being a bot playing within the match. The framework has an easy solution for that - scripts.

Scripts are very similiar to normal bots. They need a config file and a python executable. The config file can be loaded by RLBotGUI like a normal bot - to enable it, just click the checkbox next to its name. The script will be automatically started and terminated by the framework.

![Enabled script in RLBotGUI](/img/scripts/screenshot.png)

Example config:
```
[Locations]
script_file = ./my_script.py
name = My Script

[Details]
description = Example script
language = python
```

Example ``my_script.py``:
```python
import time

from rlbot.agents.base_script import BaseScript
from rlbot.utils.game_state_util import GameState


# Extending the BaseScript class is purely optional. It's just convenient / abstracts you away from
# some strange classes like GameInterface
class MyScript(BaseScript):
    def __init__(self):
        super().__init__("My Script")

    def run(self):
        # state setting
        self.set_game_state(GameState(console_commands=["Stat FPS"]))

        while True:
            time.sleep(0.5)

            # updating packet
            packet = self.get_game_tick_packet()

            # rendering
            color = self.renderer.white()
            text = f"seconds_elapsed : {packet.game_info.seconds_elapsed}"
            self.game_interface.renderer.begin_rendering()
            self.game_interface.renderer.draw_string_2d(20, 200, 2, 2, text, color)
            self.game_interface.renderer.end_rendering()


# You can use this __name__ == '__main__' thing to ensure that the script doesn't start accidentally if you
# merely reference its module from somewhere
if __name__ == "__main__":
    script = MyScript()
    script.run()
```

This script will render some text on the screen every 0.5 seconds.

If you want your loop to be run every tick, you might want to use a blocking call instead of ``time.sleep`` like this:
```python
while True:
   packet = self.wait_game_tick_packet()
   ...
```

For all available methods in the ``BaseScript`` class and their descriptions, see [its implementation here](https://github.com/RLBot/RLBot/blob/master/src/main/python/rlbot/agents/base_script.py).

[Another fully functional script example](https://github.com/RLBot/RLBot/tree/master/src/test/python/agents/script)
