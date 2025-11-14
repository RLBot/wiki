The RLBot framework support the Hoops game mode. This page contains useful values specifically about Hoops.

To make RLBot start a Hoops game you have to set the following values in `match.toml`:

```toml
[match]
game_mode = "Hoops"
# DunkHouse - "HoopsStadium_P"
# TheBlock - "HoopsStreet_P"
game_map_upk = "HoopsStadium_P"
```

## Arena

![](/img/hoops/hoops-dimensions.png)

Hoops is usually played on the map Dunk House or The Block. The dimensions for the arena are:

- Floor z: 0
- Side wall x: ±2966.67
- Back wall y: ±3581
- Ceiling z: 1820
- The diagonal walls intersect the x and y axes at: ±5782
- Wall bottom ramp radius: 172

### Goals

In Hoops, the goals are two rings. The rings are semi-circles placed (0, ±2969, 364) with radius 655 and they are connected to the wall with straight lines (except a small curve near the walls). The pipe that makes up the ring has a diameter of approximately 42.


### Boost Pads

There are 20 boost pads on a Hoops arena, 6 of them are big.

Big boost pads are located at (±2176, ±2944, 8) and (±2432, 0, 8).

Locations of all boost pads:

```
[ 2176, -2944, 8] (0)
[-2176, -2944, 8] (1)
[    0, -2816, 0] (2)
[-1280, -2304, 0] (3)
[ 1280, -2304, 0] (4)
[-1536, -1024, 0] (5)
[ 1536, -1024, 0] (6)
[  512,  -512, 0] (7)
[ -512,  -512, 0] (8)
[-2432,     0, 8] (9)
[ 2432,     0, 8] (10)
[  512,   512, 0] (11)
[ -512,   512, 0] (12)
[-1536,  1024, 0] (13)
[ 1536,  1024, 0] (14)
[-1280,  2304, 0] (15)
[ 1280,  2304, 0] (16)
[    0,  2816, 0] (17)
[-2176,  2944, 8] (18)
[ 2176,  2944, 8] (19)
```

Note: Some of these coordinates have been rounded.

## Ball

The radius of the ball is 98.38. When the kickout countdown hits zero, the ball is given a Z velocity of 1000. But other than that, the physics of the ball is similar to soccar.

## Spawning

| Kickoff         | Blue                              | Orange                            |
|-----------------|-----------------------------------|-----------------------------------|
| Right corner    | loc: (-1536, -3072), yaw: 0.5 pi  | loc: (1536, 3072), yaw: -0.5 pi   |
| Left corner     | loc: (1536, -3072), yaw: 0.5 pi   | loc: (-1536, 3072), yaw: -0.5 pi  |
| Back right      | loc: (-256, -2815), yaw: 0.5 pi   | loc: (256, 2815), yaw: -0.5 pi    |
| Back left       | loc: (256, -2815), yaw: 0.5 pi    | loc: (-256, 2815), yaw: -0.5 pi   |
| Far back center | loc: (0, -3200), yaw: 0.5 pi      | loc: (0, 3200), yaw: -0.5 pi      |

| Demolished      | Blue                              | Orange                            |
|-----------------|-----------------------------------|-----------------------------------|
| Right corner    | loc: (-1152, -3072), yaw: 0.5 pi  | loc: (1152, 3072), yaw: -0.5 pi   |
| Left corner     | loc: (1152, -3072), yaw: 0.5 pi   | loc: (-1152, 3072), yaw: -0.5 pi  |
