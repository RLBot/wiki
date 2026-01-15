# Game Data

The majority of the communication between RLBot and its clients consists of MatchConfigurations, FieldInfos, GamePackets, BallPrediction, and Controllers.

All supported languages have access to the same data, though syntax and conventions will vary.
The exact data can be found in the flatbuffer schema [gamedata.fbs](https://github.com/RLBot/flatbuffers-schema/blob/main/schema/gamedata.fbs).
The schema data is well-documented and explains most game data in detail.

In short:

- **MatchConfiguration:** Contains the details about the match, its participants, its mutators, etc. It is sent to all clients on match start.
- **FieldInfo:** Contains static information about the map, including boost pad locations and goal locations. It is sent to all clients on match start.
- **GamePacket:** Contains live game data, such as ball position and velocity, player positions and velocity, boost pad timers, current scores and time left, and much more. It is send to all clients at 120 Hz.
- **BallPrediction:** A highly accurate array of the ball's coming states, assuming no car hits it. It is sent alongside the game packet. See [BallPrediction](/v5/botmaking/ball-path-prediction/).
- **Controller:** Contains a combination of pressed buttons. The bots send this to the RLBot server as often as possible in reponse to the GamePacket. If a tick is missed, the controller from previous tick is used.

### Language-Specific Guides

**V4:**

- [Python](https://github.com/RLBot/RLBotPythonExample/wiki/Input-and-Output-Data)
- [Java](https://github.com/RLBot/RLBotJavaExample/wiki/Input-and-Output-Data)
- [C#](https://github.com/RLBot/RLBotCSharpExample/wiki/Input-and-Output-Data)
- [Rust](https://docs.rs/rlbot/0.5.0/rlbot/#structs)
- [Nim](https://github.com/RecruitMain707/NimExampleBot/wiki/Data-structure)
