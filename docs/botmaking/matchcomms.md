## Matchcomms allows communication between participants in a match.

Use cases:
* Telling your teammate where you intend to go
* Setting parameters on bots in a training exercise
* Bootstrapping a more efficient form of communication

Currently only broadcasting to all participants is supported.

### Architecture
![matchcomms architecture](/img/matchcomms/architecture.png)

### Examples
[Python example for reading and replying to messages in a bot.](https://github.com/RLBot/RLBotTraining/blob/master/rlbottraining/example_bots/tweak_demonstration_bot/tweaked_bot.py)

[Example training exercise which sets parameters on the bot](https://github.com/RLBot/RLBotTraining/blob/master/tests/test_on_briefing_tweak.py)

### TMCP

*TMCP* stands for the *Team Match Communication Protocol*. It is an attempt at standardizing inter-bot communication via the match comms system.

[See this wiki page for implementation details](/botmaking/team-match-communication-protocol).

### Supported Languages
- Python has strong support, as described above
- Java bots will receive args like `--matchcomms-url ws://localhost:53970` when starting up. No library support yet for creating / managing the connection.
- C# bots will receive args like `--matchcomms-url ws://localhost:53970` when starting up. No library support yet for creating / managing the connection.