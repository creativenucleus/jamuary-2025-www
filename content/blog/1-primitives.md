---
title: "1: Primitives"
description: "Spicing up the basic shapes"
publishDate: 2025-01-01
---
Shortcuts:
[The Basic Shapes](#the-basic-shapes)
| [Jazz it Up!](#jazz-it-up)
| [A Dragon](#a-dragon)
| [Dioramas](#dioramas)

---

I'm going to give the honour of first tip to:

> Have [the TIC-80 reference](https://tic80.com/learn) open in a browser tab so you can check on details.

I've done plenty of ByteJams and I still need it when I'm under pressure and things go wrong or my mind goes blank. There's no shame in that.

Your mileage will vary, but the most useful things for me have been:

- The memory map, especially the special area of VRAM (we'll explore this later)
- When something isn't working and you need to check the order of arguments. Side-eye glances at `ttri` and `memcpy` 😠

Similarly:

> If you have tiny, but complicated bits of code that you like to use, have some code snippets that you can copy and paste in. Maybe organised in a GitHub repository.

I'm sure spectators would prefer to see you acheive your vision rather than getting sunk by a typo you can't figure out for 20 minutes. This'll be frowned-upon for competitive livecoding, of course, but we're ByteJamming so turn that frowned-upon upside downed-upon!

I tend to copy in some short functions (3-5 lines) for 3D rotation and projection, because I've wrestled with those enough. One day I'm sure I'll get round to learning them solidly. (Maybe. If I need to).

## The Basic Shapes

TIC-80 has a selection of built-in primitives. You'll become familiar with them quite quickly!

`pix` draws a pixel, `line` draws a line, `circ` and `elli` draw round things, `rect` something square... maybe even a square, and `tri` draws a triangle.

Those shape functions draw solids, but each comes with a counterpart that just draws the outline, namely: `circb`, `ellib`, `rectb`, and `trib`.

We also have an extra special triangle function, `ttri` which draws textures. It's especially useful - but we'll leave that to be explored later this month.

## Jazz it up!

At some point, you'll find yourself throwing a few shapes around the screen, and this tip is:

> Take a moment to step back to explore the small things

You probably started off with a shape, wrapped it in a loop so that there's loads of them and then become consumed by how they move as a whole. But little changes to that shape can have a huge effect on the feel of your effect and sometimes these will surprise you.

You could try replacing the shape, colour application, outline, spacing, or movement:

![A selection of moving shapes](/image/1/primitives-style.gif)

Shout out to [Mantratronic](https://livecode.demozoo.org/performer/Mantratronic.html#mc), who has an excellent eye for this detail, especially outline shapes.

I find the alternating black/colour aesthetic very pleasing. Code for that looks like:

```lua
function TIC()
	cls()
	for i=56,0,-1 do
		local a=time()*.002+i*.04
		local c=(i%2==0) and 1+(i//2)%15 or 0
		circ(
			120+math.sin(a)*50,
			68+math.cos(a)*50,
			10,
			c
		)
	end
end
```

![Alternating black/colour](/image/1/alternating-colours.gif)

## A Dragon

I rarely get TIC-80 into a hang. It's pretty difficult, but there it does lock up while drawing shapes and if the shapes are huge, then that can be a problem. The following code locks TIC for a few seconds on my machine:

```lua
function TIC()
	cls()
    -- That's a big circle!
	circ(120,68, 100000000, 12)
	exit()
end
```

Yep, that's a VERY large circle, about half-a-million times the width of the screen. You wouldn't expect to type that, but I've made it happen accidentally (e.g. when dividing 1 by a very small fraction, or by sending a number to a large power `x^y`). That can happen more easily we get to playing with 3D and/or randomising values, so if you feel you're in danger of locking your own (and the server) TIC, then use something like `math.min(r, 500)` or similar to cap the size!

If it happens, it's likely not the end of everyone on the stream because each TIC will probably be CPU isolated from the others, but it will be a hassle to sort out on both client and server sides.

## Dioramas

A mastery of basic shapes is very useful for drawing scenes.

I'm not so hot at that, so can't offer my own suggestions, but I'd like to recommend you browse [Enfys'](https://livecode.demozoo.org/performer/T%25C3%25B4Bach.html#mc) archive. Many of their effects use TIC-80's default cartoony palette, but with excellent attention to the edges for better 'pop', and also the smaller details that make something characterful.