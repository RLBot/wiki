## Startup

![](/img/architecture/general.png)

## Bots

### Python Bot

![](/img/architecture/python.png)

### Java Bot

![](/img/architecture/java.png)

## Diagram Source Code

Created at sequencediagram.org with:

```
title RLBot Startup Sequence


[->Python: start match on map x with bots [y, z]
box over Python: Start RLBot.exe
box over Python: Start Rocket League
parallel
Python->DLL: start match on map x with cars [y, z]
DLL->RLBot.exe: start match on map x with cars [y, z]
RLBot.exe -> Rocket League: start match on map x
parallel off
RLBot.exe -> Rocket League: spawn cars
box over RLBot.exe,Rocket League:Continuous sync

parallel
Python->DLL: poll for game tick packet
DLL->RLBot.exe: poll for game tick packet
parallel off
parallel
RLBot.exe->DLL: packet with active match
DLL->Python: packet with active match
parallel off
box over Python:Launch bot processes y and z,\ninforming each of their index
```

```
title RLBot Python Bot


[->Python BotManager: start up with index n
Python BotManager->Python Bot:construct with index n
parallel 
Python BotManager->DLL: poll for game tick packet
DLL->RLBot.exe: poll for game tick packet
parallel off
parallel
RLBot.exe->DLL: game tick packet
DLL->Python BotManager: game tick packet
parallel off
Python BotManager->Python Bot: get_output()
Python Bot->Python BotManager: SimpleControllerState
parallel
Python BotManager->DLL: send controls
DLL->RLBot.exe: send controls
parallel off
```

## RLBot v5

The following document can be edited after making a personal copy of this document: <https://drive.google.com/file/d/13LW0l7nzc_dbwgT20cGHywx_5vNu4RTc/view?usp=sharing>

![](/img/architecture/v5.png)
