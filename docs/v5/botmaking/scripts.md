# Scripts

Sometimes it can be useful to have access to game values, debug rendering or state setting while not being a bot playing within the match. RLBot has an easy solution for that - scripts!

Scripts are very similiar to normal bots, but instead of having a `bot.toml` they have a `script.toml` file.

??? example "Example `script.toml` file"
    ```toml
    [settings]
    agent_id = "rlbot-community/script"
    name = "My script"
    run_command = "python script.py"
    run_command_linux = "./venv/bin/python script.py"

    [details]
    description = "Made possible by RLBot"
    fun_fact = "This is a test script"
    source_link = "https://github.com/RLBot/RLBot"
    developer = "RLBot community"
    language = "C#"
    tags = []
    ```

See [Config Files](/v5/botmaking/config-files/#bot-script-config-files) for details on the `script.toml` file.

## Language-specific examples

- [Python](https://github.com/RLBot/python-interface/blob/master/tests/render_test)
- [Rust](https://github.com/RLBot/rust-interface/tree/master/rlbot/examples/high_jump_script)
- [C#](https://github.com/RLBot/csharp-interface/blob/master/Tests/TestScript)
