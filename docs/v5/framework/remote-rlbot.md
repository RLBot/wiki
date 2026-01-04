# Remote RLBot

!!! warning "Automatically starting bots"
    RLBot can't automatically start bots or scripts on remote machines. You will need to start them manually, or using custom software.
    If starting the bots manually on a remote machine, set `auto_start_bots` set to `false` in RLBot.

RLBot, by default, listens for TCP connections on `0.0.0.0` (_all interfaces on IPv4 & IPv6_) on port `23234`.
This port can be changed by launching RLBot and specifying a different port for the first argument.
For example, to have RLBot listen on port `12345` instead, you would run `RLBotServer.exe 12345`.

This means that external processes can connect to RLBot from other machines on the same network by specifying the IP address of the machine running RLBot and the port number RLBot is listening on.
By utilizing RLBot's IPv6 support, you can also connect to RLBot instances running on different networks over the internet, provided that the necessary port forwarding and firewall configurations are in place.

!!! warning "RLBot on untrusted networks"
    Exposing RLBot to untrusted networks may pose significant security risks.
    Malicious actors could exploit currently unknown vulnerabilities in RLBot or the underlying system.
    It is crucial to implement robust security measures, such as _**running RLBot inside a secure virtual machine**_ and _**restricting network access using firewalls**_, to mitigate potential threats.

Bots and scripts using RLBot-standard compliant interfaces will read the environment variables `RLBOT_SERVER_IP` and `RLBOT_SERVER_PORT`, which default to `127.0.0.1` and `23234` respectively.
You can override these environment variables to point to a different machine running RLBot.
They will also expect the environment variable `RLBOT_AGENT_ID` to be set, which can be found by reading the `agent_id` field in `[settings]` header of the corresponding config toml file.

??? info "What does RLBot do with the agent ID?"
    RLBot uses the given `RLBOT_AGENT_ID` to auto-negotiate which car(s) bots will be able to control - with an incorrect one, the connection will be rejected by RLBot.
    After the connection is established, bots will only be able to send car inputs for their assigned car and any car inputs sent by scripts will be ignored.
    This ensures fair matches and prevents bots from interfering with each other in competitive settings.
    It does NOT prevent extra connections to RLBot from being established and reading game state data, so be cautious about exposing RLBot to untrusted networks.
