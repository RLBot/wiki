To make RLBot start a Dropshot game you have to set the following values in `match.toml`:

```toml
[match]
game_mode = "Dropshot"
game_map_upk = "ShatterShot_P"
```

??? warning "RLBot doesn't have access to individual tile data"
    RLBot v5 currently doesn't have access to the position or state information of individual dropshot tiles. This is a known issue that was present in RLBot v4, but this information might return.

## Tiles

There are 140 tiles in total.

A list of all locations can be found [here](https://pastebin.com/w79VhU8W).

The tiles are hexagonal in shape. The distance between centres is 768 uu (except for the middle where the distance from blue tiles to orange tiles is 256 uu. A neutral strip about 128 uu wide covers the tiles partially). The image below shows the relative dimensions of a Dropshot tile.

![A Dropshot tile's dimensions](/img/dropshot/tiles.png)

## Goals

There are 2 goals under the floor, one per team.

Locations and owning team can be found in the agent's FieldInfo as GoalInfo objects. In Python `self.field_info.goals` will be a list of the goals with following attributes:

```python
class GoalInfo:
    team_num: int
    location: Vector3
    direction: Vector3
    width: float
    height: float
```

Example: To access the location of tile 0 in Python, write: `self.field_info.goals[0].location`

The goals provided are as follows:

```rust
[
    GoalInfo {
        team_num: 0,
        location: Vector3 {
            x: 0.0,
            y: -4430.0,
            z: -1040.0,
        },
        direction: Vector3 {
            x: -8.742278e-8,
            y: -0.0,
            z: 1.0,
        },
        width: 5408.0,
        height: 12640.0,
    },
    GoalInfo {
        team_num: 1,
        location: Vector3 {
            x: 0.0,
            y: 4430.0,
            z: -1040.0,
        },
        direction: Vector3 {
            x: -8.742278e-8,
            y: -0.0,
            z: 1.0,
        },
        width: 5408.0,
        height: 12640.0,
    },
]
```

## Arena

The tiles are layed out in rows of `7,8,9,10,11,12,13 |middle| 13,12,11,10,9,8,7`. Where tile 0 is in the blue corner (back left if you are on blue team) and tile 139 is in an orange corner (back left if you are on the orange team).

The overall layout of the arena and tile indices can be seen below.

![Layout of Core707](/img/dropshot/arena.png)

The arena is a regular hexagon except its rounded corners. Here are some dimensions:

- Center: (0, 0)
- Floor level: 3.2 uu
- Center to wall: 4555 uu
- Center to corner: 5026 uu
- Center to corner (without rounding walls): 5259.66 uu
- Wall length: about 5026 uu
- Arena height: 1986 uu

Note that the tiles on the boundaries are not complete - the rounded base of the walls overlaps the tiles to varying degrees (approximately 30-40% of the tile is overlapped, and the tile centrepoints are always visible).

You may notice that tiles' locations are at z=0, but floor level is z=3.2. This is because the tiles have a thickness.

#### Within arena script

Here's a small script to check if a point is within the arena:

```python
def is_within_arena(point: Vector3) -> bool:
	HEIGHT = 1986
	TO_WALL = 4555
	TO_CORNER = 5260  # 4555 / math.sin(60)
	
	if point.z < 0 or HEIGHT < point.z:
		return False
	
	ax = abs(point.x)
	ay = abs(point.y)
	
	if TO_CORNER < ax or TO_WALL < ay:
		return False
	
	return TO_WALL * TO_CORNER - TO_WALL * ax - 0.5 * TO_CORNER * ay >= 0;
```

## Ball

Ball radius: 102.24 uu

The ball has three phases.

- 0: Normal
- 1: Charged
- 2: Super Charged

The ball gains charge as it is hit by cars. When a car exerts a force on the ball, the ball charges up equal to the magnitude of the force (energy). The ball enters the next phase when it reaches a certain threshold:

- Charged: 2500 energy
- Super Charged: 11000 energy

However, a hit does not always charge the ball. To make the ball charge up, the impact must be at least 500 energy, otherwise the ball will not charge at all. Furthermore, when the ball gets hit, it will temporarily accept less energy. The variable `force_accum_recent` stores the accumulated absorbed force (energy), but no more than 2500 energy. It decays over time at a rate of 2500 energy per second, and the ball will at most absorb `2500 - force_accum_recent` energy on a hit. Combined, this prevents players from charging the ball too quickly, limited to 2500 energy every second.

*Example: if you hit the ball with an impact of 2000 uu/s at t=0, and you hit the ball again with a force of 2000 uu/s at t=0.5, the `force_accum_recent` has decayed by 1250 and will be 750 when you hit the ball the second time, and therefore the ball will not accept more than 1750 energy, which means 250 energy gets lost.*

The ball's charge is reset, whenever it damages a floor tile owned by the opponent of whoever last touched the ball. To make damage the ball must hit the tile with a velocity of 250 uu/s or more perpendicular to the surface of contact. The side of a tile can only be hit if there's an open tile next to it, so usually the ball will hit the top of the tiles, and in those cases only the Z velocity matters, and must be 250 uu/s or more to make damage. There is also must have passed 0.2 seconds since the last time the ball damaged a tile.

At kickoff the ball is launched straight up into the air with a velocity of 1000 uu/s and reaches a height of about 847 uu in 1.54 seconds.

## Spawning

You always spawn with 100 boost.

Kickoff spawn locations are similar to soccar spawn locations, but not identical. In dropshot spawn locations are:

| Kickoff         | Blue                              | Orange                            |
|-----------------|-----------------------------------|-----------------------------------|
| Right corner    | loc: (-1867, -2379), yaw: 0.25 pi | loc: (1867, 2379), yaw: -0.75 pi  |
| Left corner     | loc: (1867, -2379), yaw: 0.75 pi  | loc: (-1867, 2379), yaw: -0.25 pi |
| Back right      | loc: (-256.0, -3576), yaw: 0.5 pi | loc: (256.0, 3576), yaw: -0.5 pi  |
| Back left       | loc: (256.0, -3576), yaw: 0.5 pi  | loc: (-256.0, 3576), yaw: -0.5 pi |
| Far back center | loc: (0.0, -4088), yaw: 0.5 pi    | loc: (0.0, 4088), yaw: -0.5 pi         |

| Demolished      | Blue                              | Orange                            |
|-----------------|-----------------------------------|-----------------------------------|
| Right corner    | loc: (-2176, -3408), yaw: 0.5 pi | loc: (2176, 3408), yaw: -0.5 pi  |
| Left corner     | loc: (2176, -3408), yaw: 0.5 pi  | loc: (-2176, 3408), yaw: -0.5 pi |

## Boost regeneration

In Dropshot you regenerate boost at a rate of about 10 boost/sec. However, you do not regen boost while boosting. When you stop boosting (or run out of boost), there is a delay of about 0.5 second before you start gaining boost again. If you are at 0 boost and boost button is pressed, the timer will reset every 0.5 seconds until the button is released.
