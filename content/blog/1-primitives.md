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

> Have [the TIC-80 reference](https://tic80.com/learn) open in a browser tab so you can check the details.

I've done plenty of ByteJams and I still need it when I'm under pressure and things go wrong or my mind goes blank. There's no shame in that.

The most useful things for me have been:

- The memory map, especially the special area of VRAM (we'll explore this later)
- When something isn't working and you need to check the order of arguments - side-eye glances at `ttri` and `memcpy` 😠

Similarly:

> If you have tiny, but complicated bits of code that you like to use, have some code snippets that you can copy and paste in. Maybe organised in a GitHub repository.

I'm sure spectators would prefer to see you acheive whatever you're heading for rather than getting bogged down in a typo you can't figure out. Frowned-upon for competitive livecoding, but ByteJamming is about fun, so whatever the opposite of frowned-upon is!

I tend to copy in some short functions (3-5 lines) for 3D rotation and projection, because I've wrestled with those enough. One day I'm sure I'll get round to learning them solidly. Maybe. If I need to.

## The Basic Shapes

TIC-80 has a selection of built-in primitives. You'll become familiar with them quite quickly!

`pix` draws a pixel, `line` draws a line, `circ` and `elli` draw round things, `rect` something square... maybe even a square, and `tri` draws a triangle.

Those shape functions draw solids, but each comes with a counterpart that just draws the outline, namely: `circb`, `ellib`, `rectb`, and `trib`.

We also have an extra special triangle function, `ttri` which draws textures. It's especially useful - but we'll leave that to be explored later this month.

## Jazz it up!

At some point, you'll find yourself throwing a few shapes around the screen, and this tip is:

> Take a moment to step back to explore the small things

You probably started off with a shape, added loads of them and then got consumed by how they move as a whole, but little changes to that shape can have a huge effect on the feel of your effect. Sometimes these may surprise you.

You could try replacing the shape, colour application, outline, spacing, or movement:

![A selection of moving shapes](/image/1/primitives-style.gif)

Shout out to [Mantratronic](https://livecode.demozoo.org/performer/Mantratronic.html#mc), who has an excellent eye for this detail, especially outline shapes.

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

Yep, that's a VERY large circle, about half-a-million times the width of the screen. You wouldn't expect to type that, but I've made it happen accidentally (e.g. when dividing 1 by a very small fraction) or by sending a number to a large power (x^y). That can happen when we get on to playing with 3D, so if you feel you're in danger of locking your own (and the server) TIC, then use something like `math.min(r, 500)` or similar to cap the size!

## Dioramas

A mastery of basic shapes is very useful for drawing scenes. I'm not so hot at that, but if that's your thing then I'd like to recommend you browse [Enfys](https://livecode.demozoo.org/performer/T%25C3%25B4Bach.html#mc) archive. Many of their effects use TIC-80's default cartoony palette, but with excellent attention to edges for pop, and smaller details to make them characterful.