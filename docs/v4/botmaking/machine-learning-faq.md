---
search:
  boost: 0.5
---

Q: I'm new to ML, where should I start?

A: There are a lot of helpful resources pinned to the #ml-discussion channel of the [RLBot discord](https://discord.gg/S2Y2Kdr). You should also join the [RLGym discord](https://discord.gg/t7xcJnvgfa), which is more specifically for ML.

______________________________________________________________________

Q: I'm new to RLBot, how do I get started with ML?

A: We strongly recommend you begin by writing a hard-coded bot to get the hang of RLBot, and coding for Rocket League. Once you feel confident, join the [RLGym discord](https://discord.gg/t7xcJnvgfa), check out the official [RLGym website](https://rlgym.github.io/) which has useful docs and guides, and look at the dedicated #machine-learning channel in the [RLBot discord](https://discord.gg/S2Y2Kdr) for helpful resources.

______________________________________________________________________

Q: What can I use ML for in Rocket League?

A: Lots of things! Most people simply use it to make bots for playing the game, but that isn't all. Optimizing specific mechanics rather than the entire game (wave-dashing, aerials, power-sliding, etc) and analyzing replays (predicting shot percentages, car positions, etc) are some other things people have done.

______________________________________________________________________

Q: How could I use the replays available at [calculated.gg](https://calculated.gg/)?

A: Tools like [Training Data Extractor](https://github.com/oxrock/TrainingDataExtractor) and [carball](https://github.com/SaltieRL/carball) exist to format replay data into an RLBot tick packet, and approximate player inputs between frames. These enable users to easily handle data available in replays for whatever purpose they might have.

______________________________________________________________________

Q: Is it possible to get controls from replays so I can train a model to copy humans?:

A: Training controls directly from replays has been used to some success, most notably with [Ripple](https://wandb.ai/rolv-arild/ripple/reports/Ripple--VmlldzozNDE5NzE4). There exist tools to find inputs from replay files, such as [RLCarInputSolver](https://github.com/ZealanL/RLCarInputSolver), and even public replay repositories (such as the one Ripple used, [here](https://www.kaggle.com/datasets/rolvarild/high-level-rocket-league-replay-dataset)) with tens of thousands of hours of data!

______________________________________________________________________

Q: Can I use a clone of Rocket League for training and then transfer it to the real game?

A: This is how most people train now! [Rocketsim](https://github.com/ZealanL/RocketSim) is used for training by basically everyone, but you don't really have to think about it, as rocketsim is used by default by current libraries.

______________________________________________________________________

Q: Can I use Supervised Learning to train a bot that mimics other bots?

A: Yes you can! The ML bots [Levi](https://github.com/SaltieRL/Saltie/tree/master/agents/levi) and [Tensorbot](https://github.com/DanielDowns/ArtificialIntelligence/tree/master/RocketLeagueBots/TensorBot) were trained this way.

______________________________________________________________________

Q: Can I use this or that method to speed up Reinforcement Learning?

A: Try it out! If it works for other problems, then it might also work for Rocket League. Don't hesitate to ask in the discord if you want a second opinion or want to know if it's been tried before.
