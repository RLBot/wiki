

### RLBot v5

00. None
    - Aka the disconnect request.
01. **[GameTickPacket](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L220-L226)**
    - Received by sessions at a high rate according to [tick rate](/botmaking/tick-rate.md)
    (except "desired tick rate" is always 120hz in v5)
02. **[FieldInfo](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L245-L249)**
    - Received by a session after it sends `ConnectionSettings`.
03. [StartCommand](https://github.com/RLBot/flatbuffers-schema/blob/main/matchstart.fbs#L320-L322)
    - Starts a new match after reading the given config file.
04. **[MatchSettings](https://github.com/RLBot/flatbuffers-schema/blob/main/matchstart.fbs#L297-L318)**
    - Received by a session after it sends `ConnectionSettings`.
    - Can be sent by sessions to start a new match.
05. [PlayerInput](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L45-L48)
    - Sent by sessions to control their cars.
06. [DesiredGameState](https://github.com/RLBot/flatbuffers-schema/blob/main/gamestate.fbs#L60-L66)
    - Sent by sessions to change the game state.
    - Ignored if state setting was disabled in the match settings.
07. [RenderGroup](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L89-L93)
    - Sent by sessions to render lines & text in the game.
    - Ignored if render setting was disabled in the match settings.
08. [RemoveRenderGroup](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L97-L99)
    - Sent by sessions to remove a render group.
    - Ignored if render setting was disabled in the match settings.
09. **[MatchComm](https://github.com/RLBot/flatbuffers-schema/blob/main/comms.fbs#L3-L14)**
    - A copy is received by sessions when another session sends the message.
    - Messages may not be received if the message was filtered due to `team_only` being requested in the message.
    - Only received if enabled in `ConnectionSettings`.
10. **[BallPrediction](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L261-L266)**
    - Received by sessions right before every `GameTickPacket` if enabled in `ConnectionSettings`.
11. [ConnectionSettings](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L8-L18)
    - Sessions send this immediately after connecting.
    - Tells the server what kind of data it wants to receive.
12. [StopCommand](https://github.com/RLBot/flatbuffers-schema/blob/main/matchstart.fbs#L324-L327)
    - Ends the current match, and optionally tells `RLBotServer.exe` to shut itself down.
13. [SetLoadout](https://github.com/RLBot/flatbuffers-schema/blob/main/matchstart.fbs#L52-L55)
    - Sent by sessions to change the loadout of their cars.
    - Will always work if a loadout was not specified in match settings and when sent before `InitComplete`.
    - Ignored if state setting was disabled in the match settings, and a loadout was set in match settings.
14. [InitComplete](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L20-L22)
    - Indicates that the session has finished all initialization and is ready to start receiving
    game messages without delay.
    - The match will not start until all sessions have sent this message.
    - `SpawnId` will be used to set the name of the session in RLBot's performance monitor and when
    filtering messages. Hiveminds should simply send the first spawn id they received from the framework.
