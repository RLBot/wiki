Bots have a function like `get_output(packet)` or something similar, depending on language. This gets called some number of times per second, I will refer to this as the tick rate.

There are several factors which influence tick rate:
1. The frame rate of Rocket League. If Rocket League is running at 60 FPS, the tick rate will get capped at 60, regardless of any other settings. Note that enabled VSync also limits FPS.
2. The desired tick rate that you've configured for your bot. Most languages allow you to add an additional cap.
3. The tick rate can never be higher than 120, since this is the rate at which Rocket League simulates its physics.

In other words, the tick rate is `min(rocketLeagueFPS, desiredTickRate)`. You can see the tick rate you're getting during a game by hitting the 'home' key and looking for a line like `ticks: 120.0 tps`

There used to be another factor, the PacketSendRate configuration. There's a file at `C:\Users\yourname\Documents\My Games\Rocket League\TAGame\Config\TARLBot.ini` where you could write down a PacketSendRate, but RLBot now ignores it and sets 240 no matter what, so it's a non-issue.


## Specifying desired tick rate

### Python
In python, you can update your bot cfg file as seen in [this example](https://github.com/RLBot/RLBot/blob/652b8c3fd94b347fdc04daeb3e2c90702be30073/src/test/python/agents/atba/atba.cfg#L9)

Python will assume your desired tick rate is 60 unless you override it. This is because TARLBot.ini is 60 by default, so the vast majority of bot makers have been developing and testing with 60.

### Java
In Java, you can configure your bot manager as seen in [this example](https://github.com/RLBot/RLBot/blob/652b8c3fd94b347fdc04daeb3e2c90702be30073/src/test/java/rlbot/JavaExample.java#L20)


Java will assume your desired tick rate is 60 unless you override it. This is for the same reason as Python, and also because Java was hard-coded to use 60 for a long time.

### .NET / C#
In .NET, you can configure your bot manager as seen in [this example](https://github.com/RLBot/RLBot/blob/652b8c3fd94b347fdc04daeb3e2c90702be30073/src/test/cs/TestBot/Program.cs#L16)


Here, 0 means that you wish to have your desired tick rate be unlimited. You may also pass numbers like 60 or 120.

## Tips
- You should probably specify a tick rate that divides into 120. Tournaments generally run at 120Hz or 60Hz, and if you specify something weird like 100Hz, your bot will get called at varying intervals to achieve that average.
- In most languages, your choice of tick rate will not affect the 'freshness' of packets delivered. They should all be very fresh, about 1ms old, when they are delivered to you.
- If your bot is slow to compute a frame, you will fall behind and potentially miss a frame, since bots generally loop on a single thread. If you desire consistency, you may wish to configure a desiredTickRate that always gives enough time for your bot logic.
- To consistently give bots the same amount of execution time for every tick, rocket leagues fps should be locked to exactly the tick rate that you want the bots to run at. This is especially important if there are many bots running, for example in a tournament.
- It is possible to lock the FPS when RLBot is active, and unlock it when you're just playing normally. See the [RLBot Specific Game Settings](/rlbot-specific-game-settings#cap-the-in-game-fps-to-120-for-rlbot-uncap-the-fps-during-normal-play) page for instructions on how to do this.