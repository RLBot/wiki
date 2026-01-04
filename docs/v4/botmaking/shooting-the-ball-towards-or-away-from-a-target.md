---
search:
  boost: 0.5
---

## This tutorial is NOT language-specific!

This tutorial is based on the systems used by [VirxERLU](https://github.com/VirxEC/VirxERLU) which is a fork of [GoslingUtils](https://github.com/ddthj/GoslingUtils).

### TOC

- [This tutorial is NOT language-specific!](#this-tutorial-is-not-language-specific)
    - [TOC](#toc)
    - [Targets and anti-targets](#targets-and-anti-targets)
    - [Direction of approach](#direction-of-approach)
    - [Ball location offset](#ball-location-offset)
    - [Aligning with the direction of approach](#aligning-with-the-direction-of-approach)
    - [Driving](#driving)

### Targets and anti-targets

Firstly, a *target* is something that we want to hit the ball towards, and an *anti-target* is something that we want to hit the ball away from.

- Targets: This will be a list of two vectors. The first vector will be the left-most target, and the second will be the right-most target.
- Anti-targets: This is the same as targets, but you swap the left-most and right-most targets **before giving them to the algorithm, and *all the following code stays the same***! This causes the algorithm to shoot the ball anywhere but between the two targets.

We're using two points instead of just one, central point that represents the target because just one point is unrealistic and restricts our options. Two points, however, gives us a range to work with and some room for error.

[Useful game values](../useful-game-values)

For example, our left-most target vector could be `[800, 5213, 321.3875]` (orange's left goal post) and our right-most target vector could be `[-800, 5213, 321.3875]` (orange's right goal post). These values have been adjusted to account for the ball's radius (which is 92.75).

### Direction of approach

Now we need to find the direction that we need to hit the ball in. It wouldn't be fun if we ended up hitting the ball away from our target!

In order to do this, we need the following information:

- Left-most target
- Right-most target
- Ball location
- Car location

Let's get started!

```c
Vector car_to_ball = ball_location - car_location
Vector car_to_ball_direction = normalize(car_to_ball)

Vector ball_to_left_target_direction = normalize(left_target_location - ball_location)
Vector ball_to_right_target_direction = normalize(right_target_location - ball_location)
```

These are a bunch of directions, and they're fairly well described just by the variable names. You will have to implement your own normalize function, but [here's the math behind it if you didn't know](http://www.fundza.com/vectors/normalize/). *Many example bots, including the ones for Python and Java, already have vector classes that have implemented this for you!*

Next, we need some way to change `car_to_ball_direction` so that it's between `ball_to_left_target_direction` and `ball_to_right_target_direction`. That's what the following function `clamp2D` does.

```c
Vector clamp2D(Vector direction, Vector start, Vector end) {
    bool is_right = dotProduct(direction, crossProduct(end, [0, 0, -1])) < 0
    bool is_left = dotProduct(direction, crossProduct(start, [0, 0, -1])) > 0

    if ((dotProduct(end, crossProduct(start, [0, 0, -1])) > 0) ? (is_right && is_left) : (is_right || is_left))
        return direction

    if (dotProduct(start, direction) < dotProduct(end, direction))
        return end

    return start
}
```

You will also have to implement your own dotProduct and crossProduct functions. [See here to learn how to find the dot product of two vectors](https://www.mathsisfun.com/algebra/vectors-dot-product.html) and [see here to learn how to find the cross product of two vectors](https://www.mathsisfun.com/algebra/vectors-cross-product.html). *Many example bots, including the ones for Python and Java, already have vector classes that have implemented this for you!*

- Similar to integer clamping, clamp2D forces a direction between `start` and `end`, such that `start` \< `direction` \< `end` in terms of clockwise rotation.
- Note that this is only in the x-axis and y-axis - clamping the z-axis isn't necessary for this problem.
- For the math nerds: Why does this work? This function relies on Rocket League's inverted X-axis (positive X is left and negative X is right). [See this page](../useful-game-values#basic-dimensions) for a top-down view of the Rocket League field and different coordinates pertaining to the field.

Now we can use the `clamp2D` function.

```c
Vector direction_of_approach = clamp2D(car_to_ball_direction, ball_to_left_target_direction, ball_to_right_target_direction)
```

This is the direction that our bot should approach the ball at.

### Ball location offset

Next, we're going to take the ball's radius as well as our direction of approach into account in order to offset our final target from the ball's location.

The basis behind this is that we will go in the opposite direction of our direction of approach, and give the direction a magnitude of 92.75 (which is the ball's radius).

```c
Vector offset_ball_location = ball_location - (direction_of_approach * 92.75)
```

### Aligning with the direction of approach

This is where the math gets into approximations. What we need to do is cause the car to slowly circle around `offset_ball_location` until our direction to the ball matches or is close to `direction_of_approach` while also getting closer and closer to the target.

```c
int side_of_approach_direction = sign(dotProduct(crossProduct(direction_of_approach, [0, 0, 1]), ball_location - car_location))
Vector car_to_ball_perpendicular = normalize(crossProduct(car_to_ball, [0, 0, side_of_approach_direction]))
Vector adjustment = abs(angle(flatten(car_to_ball), flatten(direction_of_approach))) * 2560
Vector final_target = offset_ball_location + (car_to_ball_perpendicular * adjustment)
```

- You will need to implement your own sign. This returns -1 if the value is negative, 0 if the value is 0, and 1 if the value is positive.
- Most languages have their own abs (absolute value) function built-in. [See here to learn what absolute value is](https://www.mathsisfun.com/numbers/absolute-value.html).
- You will also need to implement your own flatten and angle functions. Flatten makes the z value of a vector equal to 0. [See here to learn how to find the angle between two vectors](https://onlinemschool.com/math/library/vector/angl/). *Many example bots, including the ones for Python and Java, already have vector classes that have implemented this for you!*

### Driving

Once you have `final_target`, all you have to do is face it and drive towards it. It will automatically adjust itself as your bot starts moving.

If you want to get to the target at a specific time - for example, if you're using the ball prediction - then you need the required speed in order to get to the target at that exact time. This is fairly simple, as we just need to find the distance that we need to travel and divide by the remaining time.

```c
double distance_remaining = magnitude(final_target - car_location) // an ESTIMATION of how far we need to drive - this might be spot-on or fairly far off
double time_remaining = intercept_time - current_time

double speed_required = distance_remaining / time_remaining
```

This also has another benefit: you can easily know whether or not it's possible to reach the target in a given time. If the required speed is greater than 1410 (which is the max speed of a car without boost) or 2300 (if the car does have boost) then it's impossible to reach the target in the given time.
