# TMCP

## About

*TMCP* stands for the *Team Match Communication Protocol*. It is an attempt at standardizing inter-bot communication.

Currently, the match communication system, or match comms, is only supported by Python bots. See [Matchcomms](../matchcomms) for more information on how to use it.

Match comms do not allow for team-only messages. Until this is implemented, TMCP operates on an honor system: a bot should not look at packets from bots on another team.

## Sending packets

Bots should avoid spamming packets.
They should aim for a maximum of 10 packets per second.

**Only send a packet when something has changed.**
If you want to refine a time estimate sent in a previous message, only do so if the time difference is > 0.1 seconds.

## Receiving packets

Packets will only be received when something has changed. If you haven't received a packet from another bot, assume that nothing has changed.

______________________________________________________________________

# TMCP 1.0

This is what a packet following TMCP looks like:

```python
{
  "tmcp_version": [1, 0],
  "team": int,
  "index": int,
  "action": {
    "type": string,
    ...
  }
}
```

- `tmcp_version`

    - Major: Breaking revision number
    - Minor: Non-breaking revision number

- `team` - The team of the bot that sent the packet. 0 for blue and 1 for orange. This will be removed once match comms allow for team-only communication.

- `index` - The index of the bot that sent the packet. If you are using the hivemind, make this the index of the bot which is carrying out the action.

- `action` - The current action that the bot is taking. This is an object. The types can be seen below.

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

### "BOOST"

The bot is going for boost.

```python
{
  "type": "BOOST",
  "target": int
}
```

- `target` - Index of the boost pad the bot is going to collect.

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

### "READY"

The bot is waiting for a chance to go for the ball. Some examples are positioning (retreating/shadowing) and recovering.

```python
{
  "type": "READY",
  "time": float
}
```

- `time` - The game time when the bot could arrive at the ball (if it was to go for it). `-1` for unknown.

### "DEFEND"

The bot is in a position to defend the goal and is not planning to move up. Only use DEFEND if your bot is in place to defend, not if still en-route. If the bot decides to leave the net, signal this using either "BALL" (if going for a touch) or "READY" (if moving upfield).

A bot should use "DEFEND" to let its teammates know it is safe to move up a bit without worrying about an open net.

```python
{
  "type": "DEFEND"
}
```

# TMCP Implementations

## VirxERLU

If you use VirxERLU, your bot automatically sends out TMCP packets. To process incoming ones, add this to your bot in `main.py`:

```python
def handle_tmcp_packet(self, packet):
    super().handle_tmcp_packet(packet)
    # process the packet here
```

However, you will have to implement your own 'minimum time to ball' function. This is the default:

```python
def get_minimum_game_time_to_ball(self):
    return -1
```

It should return -1 if you can't find a shot and the game time of the shot if you could. It should be noted that all shots (except for short_shot) have a property called `intercept_time` which is the game time that they will intercept the ball at.

## TMCP Handler

Will made a [TMCP python package](https://pypi.org/project/tmcp/) with helper classes that handle everything protocol-related.

To install, just do

```
pip install tmcp
```

Then you can use it like this:

```python
from tmcp import TMCPHandler, TMCPMessage, ActionType

class MyBot(BaseAgent):
    def initialize_agent(self):
        self.tmcp_handler = TMCPHandler(self)

    def get_output(self, packet: GameTickPacket) -> SimpleControllerState:
        # Receive and parse all new matchcomms messages into TMCPMessage objects.
        new_messages: List[TMCPMessage] = self.tmcp_handler.recv()
        # Handle TMCPMessages.
        for message in new_messages:
            if message.action_type == ActionType.BALL:
                print(message.time)
        
        ...

        # You can send messages like this.
        self.tmcp_handler.send_boost_action(pad_index)

        # Or you can create them and send them more directly:
        my_message = TMCPMessage.ball_action(self.team, self.index, estimated_time_of_arrival)
        self.tmcp_handler.send(my_message)
```

## TGDSMELLS Handler

The [TGDSMELLS Handler](https://pypi.org/project/tgdsmells/) is very similar in usage to Will's TMCP Handler.

Can be installed with

```
pip install tgdsmells
```

## Under the hood

The following example shows how you could send and receive match comms without a handler. This is shown to give you a better idea of what's going on under the hood. We do still recommend that you use an existing handler instead because it will be up to date and do a lot of error checking for you.

```python
from queue import Empty

class MyBot(BaseAgent):
    def initialize_agent(self):
        self.ally_actions = {}

    def receive_coms(self):
        # Process up to 50 messages per tick.
        for _ in range(50):
            try:
                # Grab a message from the queue.
                msg = self.matchcomms.incoming_broadcast.get_nowait()
            except Empty:
                break

            # This message either isn't following the standard protocol
            # or is intended for the other team.
            # DO NOT spy on the other team for information!
            if "tmcp_version" not in msg or msg["team"] != self.team:
                continue

            # Will's handler checks to make sure the message is valid.
            # Here we are keeping it as simple as possible.

            # If we made it here we know it's information relevant to our team.
            # Let's save it in a dictionary for reference later.
            self.ally_actions[msg["index"]] = msg

    def send_coms(self):
        # Let's assume that our bot is attacking the ball and aiming for prediction slice 100.
        outgoing_msg = {
            "tmcp_version": [1, 0],
            "team": self.team,
            "index": self.index,
            "action": {
                "type": "BALL",
                # You'll likely have the ball predictions cached locally.
                # This implementation is for demonstration purposes only.
                "time": self.get_ball_prediction_struct().slices[100].game_seconds,
                "direction": [0, 0, 0]
            },
        }
        self.matchcomms.outgoing_broadcast.put_nowait(outgoing_msg)

    def get_output(self, packet: GameTickPacket):
        # Get the latest information from matchcomms.
        self.receive_coms()

        # Your bot code goes here:
        ...

        # Ideally you only send out communications when your plans change.
        # Sending 120 messages a second is just overkill and will eat into the processing time of your allies.
        # Please take proper precautions to prevent excessive spam.
        if new_plan != old_plan:
            self.send_coms()

        return self.controls
```
