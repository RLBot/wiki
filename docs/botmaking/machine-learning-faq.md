Q: I'm new to ML, where should I start?

A: There are a lot of helpful resources pinned to the ml-discussion channel of the [RLBot discord](https://discord.gg/S2Y2Kdr).

______________________________________________________________________

Q: I'm new to RLBot, how do I get started with ML?

A: We strongly recommend you begin by writing a hard-coded bot to get the hang of RLBot, and coding for Rocket League. Once you feel confident, check out the dedicated #machine-learning channel in the RLBot discord for helpful resources!

______________________________________________________________________

Q: What can I use ML for in Rocket League?

A: Lots of things! Optimizing mechanics (wave-dashing, aerials, power-sliding, etc), analyzing replays (predicting shot percentages, car positions, etc), and even playing the game.

______________________________________________________________________

Q: How could I use the replays available at [calculated.gg](https://calculated.gg/)?

A: Tools like [Training Data Extractor](https://github.com/oxrock/TrainingDataExtractor) and [carball](https://github.com/SaltieRL/carball) exist to format replay data into an RLBot tick packet, and approximate player inputs between frames. These enable users to easily handle data available in replays for whatever purpose they might have.

______________________________________________________________________

Q: Is it possible to get controls from replays so I can train a model to copy humans?:

A: Training controls directly from replays has not been very successful.

The reason for this may be that not all conditions the bot will encounter are present in replays, leading to a model being unable to handle certain conditions that were not present in the replays it was trained with.

The most successful attempt is documented [here](https://github.com/Rolv-Arild/replay-pretraining).

______________________________________________________________________

Q: Can I make a clone of Rocket League for training and then transfer it to the real game?

A: It would be quite the project to clone Rocket League, but there is no reason that would not work with a little elbow grease!

The user @Roboserg#9216 has been working on a Rocket League clone for some time, and has made very promising progress. His work is open source, and can be found [here](https://github.com/roboserg/RoboLeague).

______________________________________________________________________

Q: Can I use Supervised Learning to train a bot that mimics other bots?

A: Yes you can! The ML bots [Levi](https://github.com/SaltieRL/Saltie/tree/master/agents/levi) and [Tensorbot](https://github.com/DanielDowns/ArtificialIntelligence/tree/master/RocketLeagueBots/TensorBot) were trained this way.

______________________________________________________________________

Q: Can I use Reinforcement Learning to make a bot?

A: This is the holy grail for many ML enthusiasts in the RLBot community.

With the recent release of the Python library and Bakkesmod plugin [RLGym](https://rlgym.github.io/), Reinforcement Learning can now be used to train models directly in the game.
Some promising results have already been achieved, like [Rhobot](https://www.youtube.com/watch?v=0PZ89WIr3xw) and [Element](https://imgur.com/a/DQSUAWA).
These models take quite a lot of compute power and time to train, but they are achievable with consumer-grade hardware. The largest RLGym project is currently a distributed learning bot [Necto/Nexto](https://github.com/Rolv-Arild/Necto/).

Reinforcement Learning can also be used to train specific mechanics, like [Eagle](https://www.twitch.tv/videos/488777173) and [ElementAerial](https://imgur.com/a/kleC2Jc)

______________________________________________________________________

Q: Can I use this or that method to speed up Reinforcement Learning?

A: You can just try it out, if it works for other problems, then it might also work for Rocket League.
Don't hesitate to ask in the discord if you want a second opinion.
