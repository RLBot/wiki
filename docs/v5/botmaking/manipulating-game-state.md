# Manipulating Game States

It is possible to set many game state values, for instance positions and velocities for the cars and ball.
It is commonly referred to as state setting.

State setting is disabled during most tournaments, but it's very useful when developing and debugging your bot.

This is how you enabled state setting:

- In the GUI: Open the Extra menu and tick "Enable State Setting".
- In a match config: Set `enable_state_setting` to `true` in under the `[match]` header. See [Configuration Files](/v5/botmaking/config-files).

### Possibilities

- Set up a specific scenario for your bot to try over and over. Better than custom training, because:
    - You have control over car orientation, velocity, angular velocity, and boost amount
    - You can do it in the middle of an exhibition match with opponents
    - You can control it all from inside your bot code
- Record snapshots during a game and "rewind" if you encounter a natural situation you want to iterate on
- Hover your car in midair while you work on your orientation code
- Build wacky physics rules to change the game, e.g. making the ball bounce off invisible walls, accelerate in a certain direction, etc.
  Many meme bots use state setting to create their gimmick.

## Language-specific documentation

- [Python](https://github.com/RLBot/python-interface/wiki/Manipulating-Game-State)

**V4:**

- [Python](https://github.com/RLBot/RLBotPythonExample/wiki/Manipulating-Game-State)
- [Java](https://github.com/RLBot/RLBotJavaExample/wiki/Manipulating-Game-State)
- [C#](https://github.com/RLBot/RLBotCSharpExample/wiki/Manipulating-Game-State)
