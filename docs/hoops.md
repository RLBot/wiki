The RLBot framework support the Hoops game mode. This page contains useful values specifically about Hoops.

To make RLBot start a Hoops game you have to set the following values in `rlbot.cfg`:
```
game_mode = Hoops
game_map = Hoops_DunkHouse
```

## Arena
Hoops is always played on the map Dunk House. The dimensions for the arena are:
* Floor: z=0
* Side wall: x=2966.67
* Back wall: y=3586
* Ceiling: z=????

## Goals
In Hoops the goals are two rings. The rings are placed ??? uu above the ground. Their radius is 753 uu but they are connected to the wall with straight lines.

Center of goals: ???
Distance from back wall: ???

## Boost Pads
Locations of all boost pads:
```
????
```

## Ball
The radius of the ball is 98.38 uu. But other than that, the physics of the ball is similar to soccer.

## Spawning
| Kickoff         | Blue                              | Orange                            |
|-----------------|-----------------------------------|-----------------------------------|
| Right corner    | loc: ???, yaw: 0.25 pi            | loc: ??? yaw: -0.75 pi            |
| Left corner     | loc: ???, yaw: 0.75 pi            | loc: ???, yaw: -0.25 pi           |
| Back right      | loc: ???, yaw: 0.5 pi             | loc: ???, yaw: -0.5 pi            |
| Back left       | loc: ???, yaw: 0.5 pi             | loc: ???, yaw: -0.5 pi            |
| Far back center | loc: ???, yaw: 0.5 pi             | loc: ???, yaw: -0.5 pi            |

| Demolished      | Blue                              | Orange                            |
|-----------------|-----------------------------------|-----------------------------------|
| Right corner    | loc: ???, yaw: 0.5 pi             | loc: ???, yaw: -0.5 pi            |
| Left corner     | loc: ???, yaw: 0.5 pi             | loc: ???, yaw: -0.5 pi            |