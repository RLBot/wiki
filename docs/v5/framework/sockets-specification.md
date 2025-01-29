# RLBot Sockets Specification

## Data Types

Bold items indicate the messages that bots/scripts/etc. can recieved from RLBot.

00. **None**
    - Aka the disconnect request. This can be sent to indicate that you want to disconnect,
    and this message will be sent back as confirmation of the disconnect.
    Ideally your bot only disconnects after this message is sent.
    If anything else happens, an error should be put out.
01. **[GamePacket](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L291-L307)**
    - Received by sessions at a high rate according to [tick rate](/v5/botmaking/tick-rate.md)
02. **[FieldInfo](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L338-L348)**
    - Received by a session after it sends `ConnectionSettings`.
03. [StartCommand](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L9-L12)
    - Starts a new match after reading the given config file.
04. **[MatchConfig](https://github.com/RLBot/flatbuffers-schema/blob/main/matchconfig.fbs#L372-L414)**
    - Received by a session after it sends `ConnectionSettings`.
    - Can be sent by sessions to start a new match.
05. [PlayerInput](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L30-L35)
    - Sent by sessions to control their cars.
06. [DesiredGameState](https://github.com/RLBot/flatbuffers-schema/blob/main/gamestatemanip.fbs#L76-L89)
    - Sent by sessions to change the game state.
    - Ignored if state setting was disabled in the match settings.
07. [RenderGroup](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L162-L171)
    - Sent by sessions to render lines & text in the game.
    - Ignored if render setting was disabled in the match settings.
08. [RemoveRenderGroup](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L175-L179)
    - Sent by sessions to remove a render group.
    - Ignored if render setting was disabled in the match settings.
09. **[MatchComm](https://github.com/RLBot/flatbuffers-schema/blob/main/comms.fbs#L3-L22)**
    - A copy is received by sessions when another session sends the message.
    - Messages may not be received if the message was filtered due to `team_only` being requested in the message.
    - Only received if enabled in `ConnectionSettings`.
10. **[BallPrediction](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L361-L367)**
    - Received by sessions right before every `GamePacket` if enabled in `ConnectionSettings`.
11. [ConnectionSettings](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L20-L33)
    - Sessions send this immediately after connecting.
    - Tells the server what kind of data it wants to receive.
12. [StopCommand](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L14-L18)
    - Ends the current match, and optionally tells `RLBotServer.exe` to shut itself down.
13. [SetLoadout](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L35-L44)
    - Sent by sessions to change the loadout of their cars.
    - Will always work if a loadout was not specified in match settings and when sent before `InitComplete`.
    - Ignored if state setting was disabled in the match settings, and a loadout was set in match settings.
14. InitComplete
    - Indicates that the session has finished all initialization and is ready to start receiving
    game messages without delay.
    - The match will not start until all sessions have sent this message.
    - There is no data associated with this message.
15. **[ControllableTeamInfo](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L56-L64)**
    - Contains information about the cars that the client can control.
    - Received by a session after it sends `ConnectionSettings`.
    - There may be more than one car in case the bot is a hivemind.
