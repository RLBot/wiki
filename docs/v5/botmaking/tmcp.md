# TMCP

*TMCP* stands for the *Team Match Communication Protocol*. It is an attempt at standardizing inter-bot communication over the match communications system, or "match comms" for short. See [MatchComms](../matchcomms) for more information on how to use it.

Specifically, TMCP puts a protocol on the `content` byte string of MatchComm messages using JSON.

The Python-specific wiki has [an example](https://github.com/RLBot/python-interface/wiki/Match-Comm#a-realistic-example) for encoding Python objects to JSON and then bytes and back without erroring on inavalid JSON from other bots.

## Sending packets

Bots should avoid spamming packets.
They should aim for a maximum of 10 packets per second.

Only send a packet **when something has changed.**
If you want to refine a time estimate sent in a previous message, only do so if the time difference is > 0.1 seconds.

TMCP messages should only be sent to your team.

## Receiving packets

Packets will only be received when something has changed. If you haven't received a packet from another bot, assume that nothing has changed.

______________________________________________________________________

# TMCP 1.0

This is what a message's content looks like following TMCP:

```python
{
  "tmcp_version": [1, 0],
  "action": {
    "type": string,
    ...
  }
}
```

- `tmcp_version`

    - Major: Breaking revision number
    - Minor: Non-breaking revision number

- `action` - The current action that the bot is taking. This is an object. The types can be seen below.

    - `type` - A string declaring type of the action. See valid types below.

If you are sending messages from a [hivemind](../hiveminds), make sure it is sent from the `index` the bot which is carrying out the action.

### "BALL"

The bot is going for the ball.

```python
{
  "type": "BALL",
  "time": float,
  "direction": [float, float, float] 
}
```

- `time` - Game time that your bot will arrive at the ball. Set to `-1` if this is unknown.
- `direction` - Anticipated normalized direction of ball travel AFTER contact is made. `[0, 0, 0]` for unknown.

Suggested quick chat for the MatchComm `display`: "I got it"

### "BOOST"

The bot is going for boost.

```python
{
  "type": "BOOST",
  "target": int
}
```

- `target` - Index of the boost pad the bot is going to collect.

Suggested quick chat for the MatchComm `display`: "Need boost"

### "DEMO"

The bot is going to demolish another car.

```python
{
  "type": "DEMO",
  "time": float,
  "target": int
}
```

- `time` - Game time that the bot will demo the other bot. `-1` for unknown.
- `target` - Index of the bot that will be demoed.

Suggested quick chat for the MatchComm `display`: "Demoing!"

### "READY"

The bot is waiting for a chance to go for the ball. Some examples are positioning (retreating/shadowing) and recovering.

```python
{
  "type": "READY",
  "time": float
}
```

- `time` - The game time when the bot could arrive at the ball (if it was to go for it). `-1` for unknown.

Suggested quick chat for the MatchComm `display`: "Ready"

### "DEFEND"

The bot is in a position to defend the goal and is not planning to move up. Only use DEFEND if your bot is in place to defend, not if still en-route. If the bot decides to leave the net, signal this using either "BALL" (if going for a touch) or "READY" (if moving upfield).

A bot should use "DEFEND" to let its teammates know it is safe to move up a bit without worrying about an open net.

```python
{
  "type": "DEFEND"
}
```

Suggested quick chat for the MatchComm `display`: "Defending"
