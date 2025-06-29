# RLBotServer API Specification

The root of the specification is the file [`rlbot.fbs`](https://github.com/RLBot/flatbuffers-schema/blob/main/schema/rlbot.fbs).

## Packet types

Packet types are named from the POV of the sender.

- [InterfacePacket](https://github.com/RLBot/flatbuffers-schema/blob/main/schema/interfacepacket.fbs) - Wraps `InterfaceMessage`, a union of all valid message types that can be sent _by_ interfaces, _to_ RLBotServer.
- [CorePacket](https://github.com/RLBot/flatbuffers-schema/blob/main/schema/corepacket.fbs) - Wraps `CoreMessage`, a union of all valid message types that can be sent _by_ RLBotServer, _to_ interfaces.

??? question "What is an interface? What is core?"
    - "Interface" refers to the layer of abstraction that sits in between the RLBotServer API and the bot/script someone wants to make. While not strictly required, it a good interface makes working with RLBot fun & easy - even without knowing anything about TCP, FlatBuffers, or the RLBotServer API.
    - "Core" refers to RLBotServer itself, which is considered the shared "core" of RLBot that implements various functions related to starting and running matches that are accessable via the RLBotServer API.

## Connecting to RLBotServer

1. Read the environment variable `RLBOT_SERVER_IP` and default to `127.0.0.1`
1. Read the environment variable `RLBOT_SERVER_PORT` and default to `23234`
1. Connect to the given IP/port via TCP.

## Packet format

This project uses [FlatBuffers](https://flatbuffers.dev/) as the data format,
prefixed with a 32-bit unsigned integer.
All packets should be big endian.

- Read the first 32 bits as an unsigned integer. This is `n`, the length of the flatbuffer in bytes.
- Read `n` bytes, and deserialize this into the correct flatbuffer according to the data type.

Replace "read" with "write" to send a packet.

## Connection handshake

*After connecting to RLBotServer via TCP*, bots and scripts must perform a handshake to get access to game data.
If this is not performed, then various functionality will be limited, for example, all player inputs will be rejected by the server.

1. Send `InterfaceMessage.ConnectionSettings`
    - The environment variable `RLBOT_AGENT_ID` will be set by RLBotServer before launching your process.
    This can be passed as `AgentId`, or a hardcoded default can be used if the environment variable was not present.
    A hardcoded default is useful during development when the process may be getting started manually.
    - For bots & scripts, `CloseBetweenMatches` should always be `true` with no alternate option.
1. Receive match information - In no guaranteed order, wait for all of the following to arrive:
    - `CoreMessage.MatchConfiguration`
    - `CoreMessage.FieldInfo`
    - `CoreMessage.ControllableTeamInfo` - sent for bots & scripts. If `AgentId` was invalid or blank, this will be empty.
    If this was intentional, continue as normal.
1. Parse `CoreMessage.ControllableTeamInfo` for your `team`, `index`(s), and `spawnId`(s).
  There will be multiple if this is a bot that was designated as a hivemind.
    - If `team` is `0` or `1`: `index` will be the index of your bot in `CoreMessage.GamePacket`
    - If `team` is `2`: `index` will be the index of your script in `CoreMessage.MatchConfiguration`
1. `identifier` can be used to find your bot/scripts's name in `MatchConfiguration`.
    - For bots and scripts, `identifier` corresponds to `playerId`/`scriptId` (respectively).
    - **DO NOT USE `index` FOR BOTS**, they are not in the correct order in `MatchConfiguration`. Using `index` is ok for scripts but `identifier` can be used for both.
1. Perform heavy initialization.
    - At this point, most information needed for bots to initialize most of their variables is known.
    Have some kind of callback that lets developers do this now.
    Delaying the creation of the bot object/class until now is also an idea.
    RLBotServer will load the map during this time, but won't start the match.
1. Send `InterfaceMessage.InitComplete`.
  This is a packet with no content, and signifies to RLBotServer that the match can be started.
1. Done, enter main control loop

## Main control loop

Requires the connection handshake to have been performed first. Depending on what was sent in `ConnectionSettings`, some of the following packets may not be sent.

- Every tick, `BallPrediction` will always be sent before `GamePacket`.
  - If `BallPrediction` is not sent, it's because it was disabled in `ConnectionSettings`
- `MatchComm` will arrive in between ticks. The sending of these packets can be disabled in `ConnectionSettings`
