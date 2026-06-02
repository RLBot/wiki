# RLBot Pack

The RLBot Pack is a collection of ready-to-use bots and scripts that any RLBot user can download and play against with a single click. It's the easiest way to discover community-made bots and the recommended source for tournament organizers and streamers.

## For Users: Getting Bots from the Pack

The botpack is built into the RLBot GUI. When you open the bot list, bots from the pack appear alongside your local bots. The GUI handles downloading, updating, and launching them automatically.

If you want to browse what's available or check the source code, the botpack lives at **[github.com/RLBot/botpack](https://github.com/RLBot/botpack)**.

## For Bot Makers: Submitting Your Bot

Everyone is welcome to submit their bot or script to the pack. The process is different from v4 — v5 uses a tool called **Bob** to build bots inside Docker containers, producing compiled executables that work without any language runtime dependencies.

### Submission Requirements

| Requirement | Details |
|---|---|
| **Open source** | Your submission must be open source and its repository must contain all necessary files and/or only use public dependencies. |
| **`bot.toml` or `script.toml`** | A properly configured config file (see [Configuration Files](/v5/botmaking/config-files)). You need a unique `agent_id` — strongly prefer the format `<yourname>/<botname>/<version>`. |
| **`bob.toml`** | A build configuration file so Bob can compile and package your bot. See the [examples repository](https://github.com/swz-git/bob-example) for language-specific setups. |
| **License** | Recommended but optional. If you don't include a `LICENSE` file, the botpack defaults to MIT. Your license must allow the botpack to use and distribute your code. |
| **Skill limit** | Your bot must be roughly Grand Champion rank or lower (at most as strong as Nexto). If your bot scores 42 goals before Nexto reaches 28 in any soccar mode, it's too strong. This prevents cheaters from abusing your bot in ranked play. |

### How Bob Works

[Bob](https://github.com/swz-git/bob) builds your bot inside a Docker container, producing a compiled binary that works on Windows and Linux without requiring users to install your language runtime. The botpack uses Bob to generate incremental patches so users only download what changed between releases.

To set up Bob for your bot:

1. Install **Docker** and make sure it's running.
2. Add a `bob.toml` to your bot's repository. The structure varies by language:

    **Python (hardcoded bot):**
    ```toml
    [[config]]
    project_name = "my-bot"
    bot_configs = ["./bot.toml"]

    [config.builder_config]
    builder_type = "python"
    build_mode = "hardcoded"
    entrypoint = "bot.py"
    requirements = "./requirements.txt"
    ```

    **Python (ML bot, e.g. RLGym):**
    ```toml
    [[config]]
    project_name = "my-ml-bot"
    bot_configs = ["./bot.toml"]

    [config.builder_config]
    builder_type = "python"
    build_mode = "rlgym"
    entrypoint = "bot.py"
    requirements = "./requirements.txt"
    ```

    **Rust:**
    ```toml
    [[config]]
    project_name = "my-rust-bot"
    bot_configs = ["./bot.toml"]

    [config.builder_config]
    builder_type = "rust"
    targets = ["x86_64-pc-windows-gnu", "x86_64-unknown-linux-musl"]
    bin_name = "my_bot"
    ```

    **C#:**
    ```toml
    [[config]]
    project_name = "my-csharp-bot"
    bot_configs = ["./bot.toml"]

    [config.builder_config]
    builder_type = "csharp"
    project = "./MyBot.csproj"
    ```

    **C++ / custom setup:**
    ```toml
    [[config]]
    project_name = "my-cpp-bot"
    bot_configs = ["./bot.toml"]

    [config.builder_config]
    builder_type = "custom"
    dockerfile = "./dockerfile"
    values = { windows_binary = "./mybot.exe", linux_binary = "./mybot" }
    ```

    See the **[bob-example repository](https://github.com/swz-git/bob-example)** for more complete, working examples for each language.

3. Test your build locally: `bob build bob.toml`
4. Submit your bot (see below).

### How to Submit

1. Fork **[github.com/RLBot/botpack](https://github.com/RLBot/botpack)**.
2. Add your bot's repository as a git submodule:
   ```
   git submodule add https://github.com/yourname/your-bot.git
   ```
3. Open a pull request. A maintainer will review your submission and, once accepted, Bob will automatically build and publish it to the pack.

??? tip "Pre-built binaries aren't accepted"
    Bob exists to ensure all bots in the pack are built in a reproducible, trustable environment. Submissions that include pre-built binaries will not be accepted — your source code must be buildable by Bob.

## Licensing

By default, submodules in the botpack fall under the MIT license. If your bot's repository includes its own `LICENSE` file, that license takes precedence — as long as it still allows the botpack to use and distribute your code.

You can request removal of your bot from the botpack at any time by opening an issue or PR on the botpack repository.
