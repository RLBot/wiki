When writing a bot, it's very useful to know where the ball will be a few seconds in the future. This is very challenging because the ball responds to gravity, drag, friction, spin, and bounces off of curvy walls. Fortunately, RLBot has your back.

!!! warning "Ball prediction is an estimation"
    RLBot's ball prediction **it is not exact**! It is recommended that you routinely update your predictions to have the most accurate estimates.

RLBot v5's ball prediction is 6 seconds' worth of ball path data, each advancing 1/120 of a second into the future for a total of 720 slices. Each slice has the predicted position, velocity, and angular velocity of the ball. It's assumed that the ball doesn't collide with any of the cars on the field.

## Message structure

[The `BallPrediction` flatbuffer](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L361-L367) defines the data in each message. The message is a list of 720 `PredictionSlice`s, with each slice advancing 1/120 of a second into the future past the previous slice.

A `PredictionSlice` contains the following fields:

- `game_seconds` - The moment in game time that this prediction corresponds to. This corresponds to 'seconds_elapsed' in [`MatchInfo`](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L254-L279).
- `physics` - The [`Physics`](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L116-L122) information for the ball at this slice. This contains:
    - `location`
    - `velocity`
    - `angular_velocity`

!!! note "Ball rotation"
    Since the ball is a sphere, its orientation doesn't matter as the shape is the same in all directions. For this reason, the `rotation` is not computed and the field is always has `0` for `pitch`, `yaw`, and `roll`.

## Language-specific examples

- [Python](https://github.com/RLBot/python-interface/wiki/Ball-Path-Prediction)
