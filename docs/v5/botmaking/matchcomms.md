# Match communication

MatchComms enables communication between participants in a match.

Use cases:

- Telling your teammate where you intend to go
- Setting parameters on bots in a training exercise
- Displaying messages to humans

## Message structure

[The `MatchComm` flatbuffer](https://github.com/RLBot/flatbuffers-schema/blob/main/comms.fbs#L3-L22) defines the data in each message:

- `index` - The index of the player that sent this message. For scripts, this value is the index in the match configuration instead.
- `team` - The team of the player that sent this message. `0` for blue, `1` for orange and `2` for scripts.
- `team_only` - `true` if this message is team-only, `false` if everyone can see it. Messages marked as `team_only` will only be sent to processes with the same `team`.
- `display` - The message that will be displayed on the screen in quick chat. This is intended for communication with humans. Use the `content` field for communication with bots and scripts.
- `content` - The contents of the message. Use the `display` field for messages in quick chat.

## Language-specific wikis

- [Python](https://github.com/RLBot/python-interface/wiki/Match-Comm)

## Quick chat

In RLBot v4, bots could send various preset messages using quick chat. This has been replaced by the `display` field in v5's MatchComms.

The following v4 quick chats map to these messages in v5:

- `Information_IGotIt = "I got it!"`
- `Information_NeedBoost = "Need Boost!"`
- `Information_TakeTheShot = "Take the shot!"`
- `Information_Defending = "Defending..."`
- `Information_GoForIt = "Go for it!"`
- `Information_Centering = "Centering!"`
- `Information_AllYours = "All yours."`
- `Information_InPosition = "In position."`
- `Information_Incoming = "Incoming!"`
- `Compliments_NiceShot = "Nice shot!"`
- `Compliments_GreatPass = "Great pass!"`
- `Compliments_Thanks = "Thanks!"`
- `Compliments_WhatASave = "What a save!"`
- `Compliments_NiceOne = "Nice one!"`
- `Compliments_WhatAPlay = "What a play!"`
- `Compliments_GreatClear = "Great clear!"`
- `Compliments_NiceBlock = "Nice block!"`
- `Reactions_OMG = "OMG!"`
- `Reactions_Noooo = "Noooo!"`
- `Reactions_Wow = "Wow!"`
- `Reactions_CloseOne = "Close one!"`
- `Reactions_NoWay = "No way!"`
- `Reactions_HolyCow = "Holy cow!"`
- `Reactions_Whew = "Whew."`
- `Reactions_Siiiick = "Siiiick!"`
- `Reactions_Calculated = "Calculated."`
- `Reactions_Savage = "Savage!"`
- `Reactions_Okay = "Okay."`
- `Apologies_Cursing = "$#@%!"`
- `Apologies_NoProblem = "No problem."`
- `Apologies_Whoops = "Whoops..."`
- `Apologies_Sorry = "Sorry!"`
- `Apologies_MyBad = "My bad..."`
- `Apologies_Oops = "Oops!"`
- `Apologies_MyFault = "My fault."`
- `PostGame_Gg = "gg"`
- `PostGame_WellPlayed = "Well played."`
- `PostGame_ThatWasFun = "That was fun!"`
- `PostGame_Rematch = "Rematch!"`
- `PostGame_OneMoreGame = "One. More. Game."`
- `PostGame_WhatAGame = "What a game!"`
- `PostGame_NiceMoves = "Nice moves."`
- `PostGame_EverybodyDance = "Everybody dance!"`
- `Custom_Toxic_WasteCPU = "Waste of CPU cycles"`
- `Custom_Toxic_GitGut = "git gud"`
- `Custom_Toxic_DeAlloc = "De-Allocate Yourself"`
- `Custom_Toxic_404NoSkill = "404: Your skill not found"`
- `Custom_Toxic_CatchVirus = "Get a virus"`
- `Custom_Useful_Passing = "Passing!"`
- `Custom_Useful_Faking = "Faking"`
- `Custom_Useful_Demoing = "Demoing!"`
- `Custom_Useful_Bumping = "BOOPING"`
- `Custom_Compliments_TinyChances = "The chances of that was 47525 to 1"`
- `Custom_Compliments_SkillLevel = "Who upped your skill level?"`
- `Custom_Compliments_proud = "Your programmer should be proud"`
- `Custom_Compliments_GC = "You're the GC of Bots"`
- `Custom_Compliments_Pro = "Are you <Insert Pro>Bot?"`
- `Custom_Excuses_Lag = "Lag"`
- `Custom_Excuses_GhostInputs = "Ghost inputs"`
- `Custom_Excuses_Rigged = "RIGGED"`
- `Custom_Toxic_MafiaPlays = "Mafia plays!"`
- `Custom_Exclamation_Yeet = "Yeet!"`
