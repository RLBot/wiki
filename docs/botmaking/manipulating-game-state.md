## Overview

We have a feature that lets you set exact positions, velocities, etc. for the ball and cars. You may be familiar with [BakkesMod](http://bakkesmod.com/), this is very similar. It won't be enabled during tournaments, but it's very useful when building and debugging your bot.

### Possibilities

- Set up a specific scenario for your bot to try over and over. Better than custom training, because:
    - You have control over car orientation, velocity, angular velocity, and boost amount
    - You can do it in the middle of an exhibition match with opponents
    - You can control it all from inside your bot code
- Build training sets for machine learning
- Record snapshots during a game and "rewind" if you encounter a natural situation you want to iterate on
- Hover your car in midair while you work on your orientation code
- Build wacky physics rules to change the game, e.g. making the ball bounce off invisible walls, accelerate in a certain direction, etc

## Language-specific documentation

- [Python](https://github.com/RLBot/RLBotPythonExample/wiki/Manipulating-Game-State)
- [Java](https://github.com/RLBot/RLBotJavaExample/wiki/Manipulating-Game-State)
- [C#](https://github.com/RLBot/RLBotCSharpExample/wiki/Manipulating-Game-State)
