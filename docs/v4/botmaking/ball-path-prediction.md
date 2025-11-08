When writing a bot, it's very useful to know where the ball will be a few seconds in the future. This is very challenging because the ball responds to gravity, drag, friction, spin, and bounces off of curvy walls. Fortunately, chip has your back. He did a meticulous study of the ball physics and built an extremely accurate model: https://samuelpmish.github.io/notes/RocketLeague/ball_bouncing/

Note: this is just an _estimate_ of what the ball will be doing in the future, **it is not exact**! It is recommended that you routinely update your predictions to have the most accurate estimates.

The fruit of that labor is now available to your bot with a simple function! Just call the function and you get 6 seconds' worth of ball positions, each advancing 1/60 of a second into the future.

### Language-specific examples

(support for more languages coming soon):

- [Python](https://github.com/RLBot/RLBotPythonExample/wiki/Ball-Path-Prediction)
- [Java](https://github.com/RLBot/RLBotJavaExample/wiki/Ball-Path-Prediction)
- [C#](https://github.com/RLBot/RLBotCSharpExample/wiki/Ball-Path-Prediction)
