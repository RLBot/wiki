# Hiveminds

Hiveminds are now an official part of RLBot in v5. To tell v5 that your bot is a hivemind, set `hivemind = true` under the `[settings]` header in your `bot.toml` file.

??? example "Example `bot.toml` for a hivemind"
    ```toml
    [settings]
    name = "Hives"
    loadout_file = "loadout.toml"
    run_command = "python bot.py"
    run_command_linux = "./venv/bin/python bot.py"
    agent_id = "rlbot-community/hives"
    # must set to true if you want the bot to be ran as a hivemind
    hivemind = true
    
    [details]
    description = "Made possible by RLBot"
    fun_fact = "This is a test bot"
    source_link = "https://github.com/RLBot/core"
    developer = "RLBot community"
    tags = []
    ```

See [Config Files](/v5/botmaking/config-files/#bot-script-config-files) for details on the `bot.toml` file.

## What is a hivemind?

By default, RLBot launches 1 process per bot. This is fine for most bots, but it can be inefficient for bots that are designed to work together. Hiveminds fix this by assigning all bots of the same type to the same process, _if they're on the same team_. This means that the same bot on different teams will still be controlled different processes.

!!! info "How RLBot groups bots into hiveminds"
    RLBot specifically uses `agent_id`s to group bots together, which is why it's important that your bot's `agent_id` is unique!

Basically, a hivemind is 1 process that controls multiple bots!

## Types of hiveminds

There are 2 common ways to implement a hivemind:

1. **Single-threaded hivemind**: The hivemind runs on a single thread, and each bot is controlled sequentially. This is the simplest way to implement a hivemind.
2. **Multithreaded hivemind**: The hivemind runs on multiple threads, and each bot is controlled concurrently. This is more complex, but can be more efficient.

    ??? warning "Multithreaded hiveminds are not supported in Python"
        Due to current limitations of the Python language, Python can't do multithreading. If you're curious why, learn more about the [Global Interpreter Lock](https://realpython.com/python-gil/)!

## Language-specific examples

- [Python](https://github.com/RLBot/python-interface/tree/master/tests/hivemind)
- [Rust](https://github.com/RLBot/rust-interface/tree/master/rlbot/examples/atba_hivemind)
- [C++](https://github.com/RLBot/cpp-interface/tree/master/examples/ATBA)
