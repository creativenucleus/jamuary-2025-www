---
title: "7: Print"
description: "All about text"
publishDate: 2025-01-07
---
Shortcuts:
[Optional Arguments](#optional-arguments)
| [Shadow](#shadow)
| [Outline](#outline)
| [Width](#width)
| [Newline](#newline)
| [Substring](#substring)
| [Scroller](#scroller)
| [Drawing with Letters](#drawing-with-letters)

---

Trying to communicate with text? TIC-80's print is particularly chunky and TIC-like. Here are a few tricks to smooth it over just a little.

## Optional Arguments

This is the version you'll use almost all the time:

```lua
print(text, x, y, colour)
```

But make sure you play with the optional arguments. Particularly useful for putting BIG letters on the screen, though unfortunately scale is an integer value, so we can't smoothly scale it (*):

(*) unless we use it as a texture on `ttri`. But that's for later.

`smallfont` is also interesting to explore - it's not so common.

```lua
print(text, x, y, colour, fixed=false, scale=1, smallfont=false)`
```

## Shadow

For a bit of visual pop, or to guard against your text disappearing when the text colour matches the background colours:

```lua
function TIC()
	cls()
	local text,x,y="ByteJamuary!",85,60
	print(text,x+1,y+1,14)	-- shadow first
	print(text,x,y,12)		-- then overprint with the main ink colour
end
```

## Outline

The nuclear option to outset your text is to draw it with a 1 pixel offset in all directions.

We're also overprinting once here, but there's lots of computer power to play with, so let's not worry about that.

```lua
function TIC()
	cls()
	local text,x,y="ByteJamuary!",85,60
	for dy=-1,1 do
		for dx=-1,1 do
			print(text,x+dx,y+dy,14)
		end
	end
	print(text,x,y,12)
end
```

For an extra flourish, play with the colour indices as well. Here's a snazzy metal embossed look, using the default palette:

```lua
function TIC()
	cls(2)
	local text,x,y="ByteJamuary!",85,60
	for dy=-1,1 do
		for dx=-1,1 do
			local c=math.min(math.max(14+dx+dy,12),15)
			print(text,x+dx,y+dy,c)
		end
	end
	print(text,x,y,12)	-- again, overprinting once ¯\_(ツ)_/¯
end
```

## Width

`print` returns the width of the text as it is printed on screen. You can use this to centre- or right- justify your text by printing it offscreen (it'll get clipped, and won't be expensive) just to get the width, which you can then use to set a position.

```lua
function TIC()
	cls()
	local w=print("ByteJamuary!",0,140)	-- print this somewhere offscreen
	print("ByteJamuary!",120-w/2,60,12)
end
```
## Newline

`\n` in a string moves printing to the next line.

```lua
function TIC()
	cls()
	print("It's\nByteJamuary!",88,50,12)
end
```

It'll be left justified, but you can do some clunky (possibly imperfect) alignment by adding spaces.

Unfortunately there's no built-in way to get a `height` the way you can for `width`

## Substring

You can use `string.sub` to pick out a substring.

The long form is `string.sub(text,i,j)`, but this can be shortened as per the following:

```lua
local T=0
function TIC()
	cls()
	local text="It's ByteJamuary!"
	local x=70
	for i=1,#text do
		local c=text:sub(i,i)
		local y=60+math.sin(T*.2+i)*10
		x=x+print(c,x,y,12)
	end
	T=T+1
end
```

## Scroller

A quick and dirty way to make a scroller is to print the whole string with an offset that gradually moves further and further off-screen to the left. Anything that isn't visible will be quickly clipped and you can have a wrapping strategy to keep it looping:

```lua
local T,WRAP=0,999999
function TIC()
	cls()
	local text="This is my ByteJamuary scroller running across the screen!"
	WRAP=print(text,240-(T)%(240+WRAP),60,12)
	T=T+1
end
```

## Drawing with Letters

Why let the primitive shapes have all the fun, when you can also use characters?

Shout out to [EvilPaul's effect in this Byte Jam](https://livecode.demozoo.org/event/2023_11_13_byte_jam_monday_night_bytes.html#mc) - which first makes a little lookup map of (more or less) increasingly heavy characters, and then sweeps across the screen with a math function that's then translated into one of those characters. Very stylish! [See it running](https://tic80.com/play?cart=3623)