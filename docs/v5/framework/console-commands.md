## Sending a Console Command

To send a console command, e.g. `Stat FPS`, you would do this in Python:

```python
self.set_game_state(console_commands=["Stat FPS"])
```

Other languages may vary in how you set game state.

## Known Console Commands

All of these are confirmed to work as intended. Please add more as you find them!

- `QueSaveReplay` - Creates a replay keyframe and causes a replay to be saved at the end of the match.
- `Set WorldInfo WorldGravityZ 0.0000001` - Zero gravity (pretty much). Setting to 0 returns it to default.
  You can also use [state setting](/botmaking/manipulating-game-state) to set gravity.
  **Warning:** Sending this command every frame will likely make your game lag!
- `Set WorldInfo TimeDilation 3` - Speeds up the game by 3 times. Can also be used to slow down the game
  with a number between 0 and 1. You can also use [state setting](/botmaking/manipulating-game-state)
  to set game speed.
- `Stat FPS` - Turns on a little in-game FPS counter. Can be turned back off with `Stat FPS 0`
- `ShowDebug PHYSICS` - Nice little text readout which shows you some values, and also the names of some
  specific classes/attributes that you can use `Set` on.
- `Pause` - Pauses the game. Send `Pause` to unpause the game. `get_output`/`GetOutput`/`getOutput` will
  still be called when the game is paused. This pause is the same as the pause when Escape is pressed.
- `ViewAutoCam` - This will set the camera to auto-cam mode.
- `ViewDefault` - Go to the default director camera mode.
- `ViewPlayer <team index> <player index>` To go a specific player POV - e.x. `ViewPlayer 0 2` to view
  the third player on blue and `ViewPlayer 1 2` to view the third player on orange.
- `CycleHUD` - The equivalent of pressing `H` to cycle through the HUD options.
- `CycleCamera` - Goes to the next camera view. For example, `ViewAutoCam` and then `CycleCamera` puts the
  camera into the free-flying mode.

## Research for More Console Commands

Try some of these!

[https://www.reddit.com/r/RocketLeagueMods/comments/4vuj0h/list_of_available_console_commands/](https://www.reddit.com/r/RocketLeagueMods/comments/4vuj0h/list_of_available_console_commands/)
