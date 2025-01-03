---
title: "2: Code Hygiene"
description: "Optimise for the right thing"
publishDate: 2025-01-02
---
Shortcuts:
[Be Kind to Your Future Self](#be-kind-to-your-future-self)
| [Style](#style)

---

## Be Kind to Your Future Self

I'm covering this one early - mostly because I think it's *extremely important* and maybe doesn't even feature on your priority list when you first start out.

> Optimise for being able to dig yourself out!

You've only got one hour, and your objective should be to hammer out as much clever code as possible, right?

Sounds great - until something goes wrong and the pressure doubles on you to figure it out.

As a life-long coder, I'm used to holding moving parts together in my head while I'm putting something together. I also recognise that when something isn't working when I'm sure it should be or if I lose concentration for a moment... then that mental form collapses and I scrabble around the code with fuzzy vision and a lot of panic.

So, I try to optimise for being able to pull myself up from an almost inevitable fall. It's good to be kind to yourself!

This makes my code longer-form and look unquestionably more structured than it needs to be, but yeap - I fully stand by this as a conscious choice.

A more architected layout also means it'll be easier for me to go back at a later date to pick a ByteJam apart if I think I can recycle it for a demo or experiment. It should also make the code a bit more approachable for any intrepid [LCDZ](https://livecode.demozoo.org/performer/jtruk.html#mc) adventurer.

These are *my* style tips. They probably won't all be right for you, but they might be worth trying on for size - I've realised over the years how wedded I've been to a particular code style, then having to use something else for a few months, before finally looking back at how I felt as self-constructed dogma.

So...

## Style

### Globals

I try to make all my globals fully uppercase. This helps me know they're important. And more usefully means I'll almost certainly not accidentally overwrite them. Typically `SNAKE_CASE` if they're multiple words.

### Casing

I'll be consistent with variable and function casing. I use `camelCase` for these. Single letter variables are absolutely fine - that is: hard to mess up - in a tight loop.

### Bracketing and line breaks

Whatever feels most natural! Cluster lists together in a way that makes it easy to visually pick apart each item, e.g.

```lua
v = {
    {x=10, y=20},
    {x=30, y=50},
}
```

Note also the final hanging comma after the last list item. This isn't syntactically necessary but it just helps for when you inevitably want to add another item.

### local keyword

`local` is actually pretty useful. It prevents scope clashes when you aren't being careful and exclusive with variables.

Consider a toy example:

```lua
function TIC()
 cls()
	x=5 -- [1] (I'd like x to be counting something)
	for i=1,10 do
		x=0 -- [2] (I'd like this x to be counting something also)
		for j=1,10 do
		    x=x+1 -- [3]
		end
	end
	print(x,0,0,12) -- [4]
end
```
That'll print 10 to the screen. `x` is assigned `5` in [1], then is reassigned `0` in [2], then is incremented ten times by [3] and the result at [4] is `10`.

If you preface [2] to be `local x=0`, it is scope-bound so that it doesn't clash with the previous one, and the result is `5`, not `10` as before.

It's a toy example, but the point is that you maybe you haven't intentionally named those variables the same and don't want them to interact. I've certainly gotten into a big hole more than once without `local`. It's super hard to figure out what's going wrong, so I scatter it everywhere.

As a little bonus, `local` makes variables a faster to access (unexpected but true... welcome to Lua-Lua-land!). More necessary if we're needing performance for writing a demo, but we can also be a good citizen here, right?

### I Love a Good Function

If I write a significant chunk of code, I'll likely split it off into a function and I'll be doing that from very early on in a ByteJam. Two big reasons for that:

- To keep cognitive load down. It's easier to reason about a block of code if you can see it all at the same time without having to scroll it.
- It means I'm tending to parameterize - cleaving out which numbers matter to drive that bit of the code. That's extra helpful when we get to the final stretch of the ByteJam, and I'll be exploring this in another daily drop.

Calling a function *does* have a processing overhead. Doing it a lot in a loop can have a performance hit, but:

> Unless you're doing something *very* extraordinary in a ByteJam, don't worry about optimising for performance. At all.