# Architecture

This is the basic overview of RLBot in use.

![](/img/architecture/v5_architecture_overview.svg)
<!-- Can be edited and downloaded at https://docs.google.com/drawings/d/1IdhJbJXvvJvwMWfuOXo3woofvGVA8N1aVxgHf9o0mXU/edit?usp=sharing -->

Matches are typically started by RLBotGUI, CleoPetra, or some other match orchestrator.
The match orchestrator may start up the RLBotServer, if needed.
Once the RLBotServer receives a MatchConfiguration, it will start up Rocket League with the `-rlbot` flag (if needed) and then the bot and script processes.
RLBotServer communicates with Rocket League through the official Psyonix API. Since the API is under NDA, a closed-source Bridge.dll hide the details of the API.
On startup bots and scripts connect to the RLBotServer and are briefed about the match, their index, and more.
Once the match is loaded, RLBotServer will send live game data to all connected clients, and optionally ball prediction and match comms.
Each bot client control their car(s) by responding to each GamePacket with PlayerInput.
To ease the implementation of bots, scripts, and match orchestrators, we provide interface libraries for various languages.

All client connections use TCP sockets. See the [socket protocol specification](/v5/framework/sockets-specification) for more details.

!!! info "Compiled clients"
	A bot/script does not have to be compiled to interact with RLBot.
	However, bots distributed in the RLBotPack is typically compiled, so users can avoid installing various dependencies.

## Core

A detailed depiction of the architecture of the RLBotServer including the communication with a client is seen below.

![](/img/architecture/v5_diagram.png)

<!-- The original drawio version of the above diagram can be found at `/img/achitecture/v5_diagram.drawio`-->
