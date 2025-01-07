---
title: "5: Time"
description: "*Taps watch*"
publishDate: 2025-01-05
---
Shortcuts:
[Moving with Time](#moving-with-time)

---

## Moving with Time

> Pick a way of moving time, and make it a global value

If you'd like to animate effects with time, there are two main ways to do that. You can:

- [1] Use the `time()` function
    - Even if there's too much processing to do in one video frame, you can keep your effect running with the same broad motion by using this time value to calculate a position.
    - This function returns time since the program start in milliseconds, and will likely needs to be divided by a big number to make it useable.
- [2] Initialise `T=0` and call `T=T+1` at the end of your loop
    - The speed of moving objects may be different between your machine and the server, if one is more capable than the other.
    - It ensures that every visual frame is drawn. If you're adding a value to object positions each frame then the shape of the movements in your effect will be the same across machines of different capabilities.

Either way, I'd recommend having a global time value (i.e. if you're using `time()`, still declare `T` and assign it once at the top of your `TIC`).

```lua
local T=0

function TIC()
    T=time()
    ...
end
```

This means that you can also use it at another level (i.e. inside a `BDR` function if you have one). Also, repeat calls to `time()` may have millisecond slippage - best not to introduce somthing too unpredictable!

If you split off other function calls within your main `TIC`, you might like to define them with a `t` argument so that you can tweak the timing of any specific parts, e.g.

```lua
local T=0
function TIC()
    T=time()*0.001

    drawSpiral(60,30, T*.5)
    drawSpiral(120,60, T*.8)
end

function drawSpiral(x,y,t)
    -- do some movement scaling by t
end
```

I tend to use `T=T+1`. I think that's mainly a hangover from `time()` not working properly on Macs for one public release of TIC, but I think that's not been an issue for a long time. It's also easier to make periodic time triggers (like: `if T%200=0 then...`), which is a nice pattern.

It is worth noting that effects can run at very framerates in different contexts - running on a laptop in a web browser seems to be the slowest.

(Thanks [HeNeArXn](https://livecode.demozoo.org/performer/HeNeArXn.html#mc) for nudges on the topic)