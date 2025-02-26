# Tick rate

Bots have a function like `get_output(packet)` or something similar, depending on the language.
This gets called some number of times per second, and this is referred to as the tick rate.

There are several factors that influence tick rate:

1. The frame rate of Rocket League. If Rocket League is running at 60 FPS,
   the tick rate will get capped at 60, regardless of any other settings. Note that enabled VSync also limits FPS.
2. The tick rate can never be higher than 120, since this is the rate at which Rocket League simulates its physics.

In other words, the tick rate is `min(rocketLeagueFPS, 120)`.

If you ever see a performance monitor pop up in-game on the left-hand side and it says anything other than `rlbot: 100%`
then RLBot is missing ticks due to a low or inconsistent frame rate.
If any individual bot shows less than 100%, then only that bot is missing ticks because its logic is running too slowly.
