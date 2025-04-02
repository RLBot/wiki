# Rendering

Rendering allows you to draw objects on screen, which can make debugging and testing your bot so much easier. For example, you could draw the predicted ball path, or draw where the bot wants to go.

## Enabling & disabling rendering

When installing the RLBotGUI, rendering will be disabled by default. You can turn it on by clicking 'Extra' and ticking 'Enable Rendering' in the GUI.

RLBot v5 doesn't have any keybinds to toggle rendering mid-match. Mid-match render toggling is a todo item as we figure out the best way to do this.

## Render anchors

A [render anchor](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L51-L61) is a point in space consisting of either or both of a world component and optionally a relative component:

- The world component is a fixed point in global coordinates.
- The relative component is given by a car or ball and includes a local offset that takes the position and orientation of the object into account. The render *will stay attached to the object they are associated with* and do not have to be updated each tick.

    !!! warning "Render anchors and object destruction"
        Renders that use a render anchor attached to an object will disappear if the object is destroyed (i.e. the car is demolished or the ball is scored).
        Make sure to handle this in your rendering logic!

### Combining world and relative components

By combining a world component and a relative component, you can, for example, attach a render to a car with the relative component, but also specify a world component to offset that render by `z=-50` from the car's position.

This will position the render underneath the car, with the render moving relative to the position but not the orientation of the car.

## Render types

- [Line3D](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L63-L68) - Draws a line between 2 points in 3D space.
- [PolyLine3D](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L70-L74) - Draws a continuous line through a series of 3D points. World coordinates only.

    ??? info "PolyLine3D is more efficient than Line3D for a series of lines"
        This is the recommended way to draw a large number of points in a continuous line, as it is more efficient than sending multiple `Line3D` messages. While this method was available in RLBot v4, it was implemented by sending a series of `Line3D` messages which defeated the purpose.

- [String2D](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L76-L96) - Draws a string in 2D space (screen space).
- [String3D](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L98-L115) - Draws a string in 3D space. This will be projected onto the screen based on the camera's orientation.
- [Rect2D](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L117-L134) - Draws a rectangle in 2D space (screen space).
- [Rect3D](https://github.com/RLBot/flatbuffers-schema/blob/main/rendering.fbs#L136-L151) - Draws a rectangle in 3D space. This will be projected onto the screen based on the camera's orientation.

## Rendering a large amount

If you try rendering a lot of things at once, you will start to notice this message:
`Too many bytes sent this tick, skipping message`

There are a few ways to get around this.

1. **Reduce the amount of things you are rendering.**
    This is the most straightforward way to reduce the amount of bytes sent.
    For instance, instead of rendering all 720 ball prediction slices, try rendering every 4th slice.
2. **Use `PolyLine3D`.**
    This method is more efficient than many Line3Ds if you are drawing a continuous line.
3. **Avoid drawing every frame.**
    If you only need to draw something once, don't draw it every frame.
    When calling `start_rendering`, you can pass in a `group_id` string.
    By default, the group id is just `"default"`.
    _Old renders are only cleared when a new render with the same group id is sent._
    Using this, you can send parts of your render every frame, and that render will persist until the match ends (or a new render from the same process gets sent with the same group id).

If you absolutely have to re-render a lot of items every frame, you're out of luck.

## Language-specific examples

- [Python](https://github.com/RLBot/python-interface/wiki/Rendering)
