---
title: "4: Sin and Cos"
description: "Magic? No, Even Better: Trigonometry!"
publishDate: 2025-01-04
---
Shortcuts:
[sin and cos](#sin-and-cos)
| [sin and abs](#sin-and-abs)
| [sin and sin](#sin-and-sin)
| [Everywhere!](#everywhere)
---

The feature set of TIC is neatly small and decently chosen. It makes it very approachable. Nevertheless, it'll be daunting until you find your go-to functions and structures.

> Practise, experiment, and explore to find the subset that will be your power tools

Without any doubt, I rely on two primary functions more than any others:

## `sin` and `cos`

Yes! These are those functions everyone uses in their school maths lessons to calculate the angles missing from careless triangles for no very obviously useful reason. And then most people never see them again.

But they're GREAT for ByteJams!

- `sin` and `cos` do the same sort of thing- they take a value of any size and give back a value between `-1` and `1`.
- As you increase the input value, the output value changes a little bit, sweeping between those two extremes. One complete cycle happens when you change the input value by `TAU` (that's `2 * PI`).
- `sin` and `cos` are offset from each other by a quarter of a rotation (that is, `PI / 2`), so when one of them is `0`, the other is `-1` or `1`.

I tend to default to `sin` if I'm just using one of them to move things around; that's what you'll see in these examples.

So, the general patten will be:

`output_number = midpoint + sin(some_changing_input_number) * range`

i.e. if we wanted a value between `0` and `100`, our terms would be:  

`v = 50 + S(whatever) * 50`

TIC-80's graphics interface is very forgiving of fractional values, so this means we usually don't have to worry when we send the result from `sin` directly into drawing functions for pixel positions or colour indices.

(Note: Some operations are a bit more picky, - such as the bitwise operators [e.g. `&`, `|`, `<<`, `>>`]. If you encounter errors from one of those, `value//1` is a handy way to knock the fractional part on a value away into the dust).

`sin` and `cos` are, as nature intended, great for drawing rotational geometry:

```lua
local M=math
local S,C,PI=M.sin,M.cos,M.pi
local TAU=2*PI

function TIC()
	cls()
	local nSpokes=10
	for i=0,nSpokes do
		local a=i/nSpokes*TAU-time()*.001
		local r=70         -- [1] instead try: local r=70+S(a)*30
		local dx=C(a)*r    -- [2] instead try: local dx=C(a*3)*l
		local dy=S(a)*r
		line(120,68, 120+dx,68+dy,12)
	end
end
```
Here, I'm using `sin` and `cos` to spread points around a circle, but it'll also go anywhere that some smooth value change would suit, e.g. you can swap in the commented out line [1] to drive the radius of the circle also, or mess about with some of the terms [2] for surprisingly happy accidents.

`TAU` is defined as `2*PI`. Both `sin` and `cos` loop around `TAU`, so `sin(0)`, `sin(TAU)`, `sin(TAU*2)` are all the same, which means we can throw increasing numbers into it and it'll smoothly wrap around.

## `sin` and `abs`

You can also combine `sin` or `cos` with `abs`. This will flip any negative values into positive ones, so we get an animated bounce:

```lua
local M=math
local S,C,PI,ABS=M.sin,M.cos,M.pi,M.abs
local TAU=2*PI

function TIC()
	cls()
	local y=65-ABS(S(time()*.005))*20
	print("ByteJamuary",21,y,12,false,3)
end
```

## `sin` and `sin`

We can add one wavy `sin` value to another to get something that still moves smoothly but isn't predictable.

```lua
local M=math
local S,C,PI=M.sin,M.cos,M.pi
local TAU=2*PI

function TIC()
    cls()
    for y=0,68,3 do
        local dx=
            S(y*.05+time()*.0035)*40
            +S(y*.03+time()*.0045)*40
        line(120+dx,68-y,120+dx,68+y,1+y%2)
    end
end
```
When I have multiple components adding together like this, I generally play with the term multipliers (`y*.05`, `time()*.0035`) until the speed/distance has a good feel - not too fast or slow. Then for the other terms, I choose values that are close to these - maybe within a third. This keeps them out of phase lock, where all values are changing by the same amount - e.g. if term values are multiples of each other they can look like they don't have much complexity or range.

## Everywhere!

Yep, I'm such a fan of `sin` and `cos` that I'll try them anywhere.

For example, if we're spreading colour indices (between 1 and 15) across pixels, we can use:

`8 + S(i) * 7`

Or if we're spreading across a red, green, or blue colour value, we can use:

`127 + S(i) * 127`

We'll come back to this later, and also how `sin` and `cos` will be an important part of basic 3D projection effects!