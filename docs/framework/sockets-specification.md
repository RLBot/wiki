Sockets are currently available with Windows and Mac support only. Linux coming soon.

You can use https://github.com/RLBot/RLBot/tree/master/src/main/cpp/RLBotInterface/src as a reference implementation in c++.

## Data Format

Unless otherwise specified, data is sent and received on the socket in this format:

- First two bytes are an integer (big-endian) which specifies the data type (see list below).
- Next two bytes are an integer (big endian) which specifies the number of bytes in the payload.
- Remaining bytes are a payload. The logic for parsing it will depend on the data type, but generally it will be binary data in [flatbuffer format](https://google.github.io/flatbuffers/). Using tools provided by Google (i.e. flatc.exe) and [rlbot.fbs](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs) you can auto-generate code in various languages suitable for writing / parsing the payload data.

## Data Types

Types expected to flow from RLBot to the client are in bold. Some are bi-directional.

01. **[Game tick packet](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L205-L212)** (arrives at a high rate according to /Tick-Rate except "desired tick rate" is not relevant here)
02. **[Field info](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L260-L263)** (sent once when a match starts, or when you first connect)
03. **[Match settings](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L807-L825)** (sent once when a match starts, or when you first connect)
04. [Player input](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L33-L36)
05. Actor mapping data (deprecated, related to [Remote RLBot](https://github.com/RLBot/RLBot/wiki/Remote-RLBot))
06. Computer id (deprecated, related to [Remote RLBot](https://github.com/RLBot/RLBot/wiki/Remote-RLBot))
07. [Desired game state](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L327-L333)
08. [Render group](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L371-L375)
09. **[Quick chat](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L466-L478)**
10. **[Ball prediction](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L514-L519)** (sent every time the ball diverges from the previous prediction, or when the previous prediction no longer gives a full 6 seconds into the future).
11. [Ready Message](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L831-L841) (clients must send this after connecting to the socket)
12. **[Message Packet](https://github.com/RLBot/RLBot/blob/d0c8031f0b2e4bbb5156b26cd74113f526c68bc9/src/main/flatbuffers/rlbot.fbs#L879-L886)**: List of messages, having one of the following types:
    - [PlayerStatEvent](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L843-L861) - Event when a player performs an action or earns an accolade recognized in statistics, e.g. BicycleHit, HatTrick, MostBoostPickups.
    - [PlayerSpectate](https://github.com/RLBot/RLBot/blob/d0c8031f0b2e4bbb5156b26cd74113f526c68bc9/src/main/flatbuffers/rlbot.fbs#L846-L851) - Spectator camera is now focusing on a new player.
    - [PlayerInputChange](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L870-L880) - Human or bot has touched their controller, e.g. turned on boost, changed steering, etc.

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

## Connecting

Prerequisites (handled automatically by RLBotGUI / the RLBot framework):

- Rocket League must be running under the `-rlbot` flag.
- RLBot.exe must be running. After it successfully connects to Rocket League, it will start listening on TCP port 23234.

1. As a client, connect to TCP port 23234. **Note: please expect to receive the port as a parameter in the future rather than hard coding**
2. Send a 'ready' message, as defined [here](https://github.com/RLBot/RLBot/blob/master/src/main/flatbuffers/rlbot.fbs#L818-L825). Be sure to use the data format explained above (two bytes for data type, two bytes for size...)

Some attributes in the ready message are not used yet. Once you're connected, you will start receiving data on the socket, with one of the data types specified above.

## Sending Data

Send data to the same TCP socket, using the same format (two bytes for type, two bytes for size). You can send match settings (starts a new match), player input, desired game state, render group, and quick chat.

Consult [google.github.io/flatbuffers](https://google.github.io/flatbuffers/) to learn how to construct the byte array for your payload in the language of your choice.

## Client Libraries

### Python

- [https://github.com/RLBot/RLBot/tree/master/src/main/python/rlbot/socket](https://github.com/RLBot/RLBot/tree/master/src/main/python/rlbot/socket)

## Who's Using Sockets

- The core RLBot framework uses sockets from Python to start matches.
- The core RLBot framework uses sockets from within RLBotInterface.dll to run bots.
- Javascript / Typescript: [easyrlbot](https://github.com/SWZ-github/EasyRLBot)
- [RLBotControllerOverlay](https://github.com/RLBot/RLBotControllerOverlay)
- [This little test script](https://github.com/RLBot/RLBot/blob/master/src/test/python/agents/script/socket_script.py)
