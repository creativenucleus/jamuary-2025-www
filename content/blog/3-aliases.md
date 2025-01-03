---
title: "3: Aliases"
description: "Keep it snappy"
publishDate: 2025-01-03
---
Shortcuts:
[Making Maths Tidier](#making-maths-tidier)
| [Aliases](#aliases)

---

## Making Maths Tidier

We don't get a lot of horizontal screen space to play with in TIC-80, so we can use a trick to make our code more readable.

> Alias the frequently used math functions

(I'll be doing this a bit in the code snippets in this series, so this suggestion should make them look more familiar) 

## Aliases

I have a small selection of aliases at the top of the file:

```lua
local M=math
local S,C,ATAN,PI,R=m.sin,m.cos,m.atan2,m.pi,M.random
local ABS,MIN,MAX=m.abs,m.min,m.max
local TAU=2*PI
local T=0
-- (The above are a my most used math functions but yours may be different)
...
```

If you didn't know, Lua's math functions are in an automatically included library, so typically you'd be calling `math.sin`, `math.abs` etc. around in your code. Lua treats functions as first-class values - which means you can assign them to variables. So here, we're assigning `math` to `M` and then also picking off select functions to further alias, such as `S` as `math.sin`.

We can also keep a couple of useful 'constants'. I have a sneaky `TAU` in there because that's very useful when `sin`ing and `cos`ing. `T` is my go-to for the global time variable.

You'd be very justified in having yours as a snippet you copy in before things start - because occasionally I've added things to it during the ByteJam, but messed up the aliasing order such that `math.sin` is aliased as `ABS` or whatever. That's *really* hard to track down 😂

I'm very terse with `S` and `C` because I use sin and cos a heck of a lot. `ABS` and `ATAN` are in full rather than either as `A` because I don't want to mix them up.

You'll notice they're all uppercase and prefixed with keyword `local`. The former is a personal style choice (as explained previously); the local thing isn't so necessary here, but it does make them run a tiny bit faster, so I've gotten used to doing that in demos where they might be being called lots of times within loops, and I've carried that habit over.