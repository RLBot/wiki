# Game Data

The majority of the communication between RLBot and its clients consists of GamePackets, FieldInfo, and Controllers.
RLBot sends GamePackets to bots and scripts at 120 Hz. Bots repond with a Controller to control their car.
FieldInfo is sent once and contains static information about the map.

All supported languages have access to the same data, though syntax and conventions will vary.
The exact data can be found in the flatbuffer schema [gamedata.fbs](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs).
The schema data is well-documented and explains most game data in detail.

### Language-Specific Guides

**V4:**

- [Python](https://github.com/RLBot/RLBotPythonExample/wiki/Input-and-Output-Data)
- [Java](https://github.com/RLBot/RLBotJavaExample/wiki/Input-and-Output-Data)
- [C#](https://github.com/RLBot/RLBotCSharpExample/wiki/Input-and-Output-Data)
- [Rust](https://docs.rs/rlbot/0.5.0/rlbot/#structs)
- [Nim](https://github.com/RecruitMain707/NimExampleBot/wiki/Data-structure)
