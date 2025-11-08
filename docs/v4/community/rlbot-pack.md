# RLBot Pack

The RLBotPack is a repository holding frequently requested bots in one convenient place. The main intended users are people joining the discord looking for a fun bot to try out. It may also be useful as a way for people running streams to get an up-to-date version of your bot.

To be a part of the pack, you just need to create a pull request with your bot to [the repository](https://github.com/RLBot/RLBotPack).

To qualify:

- Your bot must be capable of auto-run
    - Use Python or Rust? You're all set.
    - Other languages? We have a [Java wiki](https://github.com/RLBot/RLBotJavaExample/wiki/Auto-Launching-Java), a [C# wiki](https://github.com/RLBot/RLBotCSharpExample/wiki/Auto-Launching) and a [Scratch video](https://www.youtube.com/watch?v=4oK44syo49Y).
- Your bot must not have any crazy external requirements (except for very common ones like Java or Google Chrome)
- You must be willing to trim out any extra bot configs from your folder that people wouldn't care about
- Obey the [guidelines](../community-guidelines)
- **If you join the bot pack, people will be free to play against you for rank in the [braacket league](https://braacket.com/league/rlbot) whenever they want!**

After you create the pull request, somebody will come along, review your bot and eventually merge it to the pack.

## Pull Request Tips

- If you're uploading the files via GitHub, please make sure you're not uploading useless files like `__pycache__`, because the web upload bypasses `.gitignore`.

### Updating an existing bot

It's very common for people's forked repository to get out of sync with the central RLBot/RLBotPack, and then when you make a pull request there's a bit of a mess. Here's a strategy for avoiding that:

1. Make sure you have latest information from the central repo

```
git remote add rlbot-origin https://github.com/RLBot/RLBotPack.git
git fetch rlbot-origin
```

2. Pick a branch name for your change, e.g. my-new-change

```
git checkout -b my-new-change rlbot-origin/master
```

3. Make your changes, e.g. you may be copying / overwriting files in your bot folder.
4. Make a commit and push it to your forked repo.

```
git add .
git commit -m "My new change."
git push origin my-new-change
```

5. Visit your forked repo on github.com and create a pull request from the new branch.
