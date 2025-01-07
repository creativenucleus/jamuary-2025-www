---
title: "6: Modulo"
description: "Fifty stories tall. Striding through the land. MODULO!"
publishDate: 2025-01-06
---
Shortcuts:
[Modulo](#modulo)
| [Colours](#colours)
| [Time Triggers](#time-triggers)
| [Spatial Wrapping](#spatial-wrapping)
| [Fakey Randomness](#fakey-randomness)

---

## Modulo

Modulo wraps values, so (`v % m`) returns a value wrapped over the range `0` to `m - 1`.

There are a few circumstances where it's a very solid tool for TIC, and extra useful for ByteJamming where the visuals are fed by generated values.

## Colours

It's very useful for keeping colours tamed:

```lua
local T=0
function TIC()
	cls()
	for i=0,80 do
		local c = 8 + i%4	-- blues
		circ(i*3, 30+i, 20, c)
	end
	T=T+1
end
```

## Time Triggers

It's great for making something happen periodically:

```lua
local T=0
function TIC()
	cls()
	for i=0,80 do
		local r=20
		if T%50<10 then
			r=40	-- blip the radius
		end
		circ(120, 68, r, 12)
	end
	T=T+1
end
```

We'll have a look at using this to shuffle values in a later tip.

## Spatial Wrapping

It's neat for keeping things wrapping around the screen:

```lua
local T=0
function TIC()
	cls()	
	circ(T%280-20, 68, 20, 12)
	T=T+1
end
```

## Fakey Randomness

There's another cute pattern that allows you to make a bunch of values that look they're generated randomly with a fixed seed, and without needing to worry about maintaining a list of objects. It takes the form:

`( (i + shift) ^ spread ) % wrap`

...where `i` is the index of your items, `wrap` represents one more than the largest value you'd like back, `spread` is whatever smallish number you can find that nicely scatters the values, and `shift` helps stops `i == 0` returning `0`.

In practise, this will look like:

```lua
function TIC()
	cls()	
	for i=0,100 do
		local x=((i+100)^5.3)%240
		local y=((i+100)^5.8)%136
		pix(x, y, 12)
	end
end
```

The spread terms may need some care because they can return very regular-looking patterns if the maths all lines up.