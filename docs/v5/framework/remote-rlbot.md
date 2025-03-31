# Remote RLBot

!!! warning "Automatically starting bots"
    RLBot can't automatically start bots or scripts on remote machines. You will need to start them manually, using custom software.
    For this process to work as intended, you should still have `auto_start_bots` set to `true` in RLBot because you the bots will be started automatically, just not by RLBot itself. For each player and script in the match, ensure `run_command = ""`. This will ensure RLBot still waits for all processes to connect. You can also use `run_command` to run your custom software, which you can then use to start your bots or scripts on the remote machine.

RLBot, by default, listens for TCP connections on `0.0.0.0` (all interfaces) on port `23233`. This port can be changed by launching RLBot and specifying a different port for the first argument. For example, to have RLBot listen on port `12345` instead, you would run `RLBotServer.exe 12345`.

This means that extenal processes can connect to RLBot from other machines on the same network by specifying the IP address of the machine running RLBot and the port number RLBot is listening on.

Bots and scripts using RLBot-standard compliant interfaces will read the environment variables `RLBOT_SERVER_IP` and `RLBOT_SERVER_PORT`, which default to `127.0.0.1` and `23233` respectively. You can override these environment variables to point to a different machine running RLBot. They will also expect the environment variable `RLBOT_AGENT_ID` to be set, which can be found by reading the `agent_id` field in `[settings]` header of the corresponding config toml file.
