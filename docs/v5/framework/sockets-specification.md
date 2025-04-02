# RLBot Sockets Specification

## Data Types

Bold + italicized items indicate the messages that bots/scripts/etc. can received from RLBot.

00. ***None***
    - Aka the disconnect request. This can be sent to indicate that you want to disconnect,
    and this message will be sent back as confirmation of the disconnect.
    Ideally your bot only disconnects after this message is sent.
    If anything else happens, an error should be put out.
01. ***[GamePacket](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L291-L307)***
    - Received by sessions at a high rate according to [tick rate](/v5/botmaking/tick-rate.md)
02. ***[FieldInfo](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L338-L348)***
    - Received by a session after it sends `ConnectionSettings`.
03. [StartCommand](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L9-L12)
    - Starts a new match after reading the given config file.
04. ***[MatchConfiguration](https://github.com/RLBot/flatbuffers-schema/blob/main/matchconfig.fbs#L490-L532)***
    - Received by a session after it sends `ConnectionSettings`.
    - Can be sent by sessions to start a new match.
05. [PlayerInput](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L30-L35)
    - Sent by sessions to control their cars.
06. [DesiredGameState](https://github.com/RLBot/flatbuffers-schema/blob/main/gamestatemanip.fbs#L76-L89)
    - Sent by sessions to change the game state.
    - Ignored if state setting was disabled in the match settings.
07. [RenderGroup](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L168-L177)
    - Sent by sessions to render lines & text in the game.
    - Ignored if render setting was disabled in the match settings.
08. [RemoveRenderGroup](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L181-L185)
    - Sent by sessions to remove a render group.
    - Ignored if render setting was disabled in the match settings.
09. ***[MatchComm](https://github.com/RLBot/flatbuffers-schema/blob/main/comms.fbs#L3-L22)***
    - A copy is received by sessions when another session sends the message.
    - Messages may not be received if the message was filtered due to `team_only` being requested in the message.
    - Only received if enabled in `ConnectionSettings`.
10. ***[BallPrediction](https://github.com/RLBot/flatbuffers-schema/blob/main/gamedata.fbs#L361-L367)***
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
15. ***[ControllableTeamInfo](https://github.com/RLBot/flatbuffers-schema/blob/main/rlbot.fbs#L56-L64)***
    - Contains information about the cars that the client can control.
    - Received by a session after it sends `ConnectionSettings`.
    - There may be more than one car in case the bot is a hivemind.

## Connecting to RLBotServer

1. Read the environment variable `RLBOT_SERVER_IP` and default to `127.0.0.1`
1. Read the environment variable `RLBOT_SERVER_PORT` and default to `23234`
1. Connect to the given IP/port via TCP.

## Message format

This project uses flatbuffers as the data format,
prefixed with a 2 32-bit unsigned integer.
All messages should be big endian.

- Read the first 32-bit unsigned integer. This is the data type.
- Read the second 32-bit unsigned integer. This is `n`, the length of the flatbuffer in bytes.
- Read `n` bytes, and deserialize this into the correct flatbuffer according to the data type.

Replace "read" with "write" to send a message.

## Connection handshake

*After connecting to RLBotServer via TCP*, bots and scripts must perform a handshake to get access to game data.
If this is not performed, then various functionality will be limited, for example, all player inputs will be rejected by the server.

1. Send a `ConnectionSettings`
    - The environment variable `RLBOT_AGENT_ID` will be set by RLBotServer before launching your process.
    This can be passed as `AgentId`, or a hardcoded default can be used if the environment variable was not present.
    A hardcoded default is useful during development when the process may be getting started manually.
    - For bots & scripts, `CloseBetweenMatches` should always be `true` with no alternate option.
1. Receive match information - In no guaranteed order, wait for all of the following to arrive:
    - `MatchConfiguration`
    - `FieldInfo`
    - `ControllableTeamInfo` - sent for bots & scripts. If `AgentId` was invalid or blank, this will be empty.
    If this was intentional, continue as normal.
1. Parse `ControllableTeamInfo` for your `team`, `index`(s), and `spawnId`(s).
  There will be multiple if this is a bot that was designated as a hivemind.
    - If `team` is `0` or `1`: `index` will be the index of your bot in `GamePacket`
    - If `team` is `2`: `index` will be the index of your script in `MatchConfiguration`
1. `spawnId` can be used to find your bot/scripts's name in `MatchConfiguration`.
    - **DO NOT USE `index` FOR BOTS**, they are not in the correct order in `MatchConfiguration`. Using `index` is ok for scripts but `spawnId` can be used for both.
1. Perform heavy initialization.
    - At this point, most information needed for bots to initialize most of their variables is known.
    Have some kind of callback that lets developers do this now.
    RLBotServer will load the map but won't start the match while bots are initializing.
1. Send `InitComplete`.
  This is a message with no content, and signifies to RLBotServer that the match can be started.
1. Done, enter main control loop

## Main control loop

Requires the connection handshake to have been performed first. Depending on what was sent in `ConnectionSettings`, some of the following packets may not be sent.

- Every tick, `BallPrediction` will always be sent before `GamePacket`.
  - If `BallPrediction` is not sent, it's because it was disabled in `ConnectionSettings`
- `MatchComm` will arrive in between ticks. The sending of these messages can be disabled in `ConnectionSettings`
