---
title: "TIC-80, Remote Connections, and Joining a ByteJam"
description: "How to Get Started!"
publishDate: 2024-12-31
---
Shortcuts: [TIC-80](#TIC-80) | [How Remote Connections Work](#how-remote-connections-work) | [Joining a ByteJam](#joining-a-bytejam)

---

This is the prelude post, here to get you up and running. I'm hoping it might be useful for future reference too.

Subsequent posts will be shorter, and focused on code... I promise!

## TIC-80

TIC-80 is a 'Fantasy Console', like PICO-8 and Picotron.

It pretends to be retro-style platform, and has capabilities that make it look and sound like an early-90s console. Primarily it was designed for people to make games on, but the accessibility and style of it make it attractive to demo makers.

One important thing to note is that TIC-80 has no CPU limit - it runs as fast as it can on the host computer. This means that it may run at different speeds in different contexts (e.g. on your computer, on the web, on someone else's computer etc.). This lack of common baseline performance can cause some issues, but I think it slides it closer towards being an art tool... and that's no bad thing for us ByteJammers.

TIC's visual capabilities and default palette can give effects a tell-tale chunky/cartoon look - the palette is very 'The Simpsons', but it doesn't have to look like that, and we'll explore this in subsequent tips.

## How Remote Connections Work

TIC-80 is open source [on GitHub](https://github.com/nesbox/TIC-80) and has been augmented by the livecoding community. These modifications are in that codebase, but full releases don't come frequently, so we currently use a special fork of the code with these changes. The main upgrades are:

1. TIC-80 can be started with a flag that tells it to dump the code from the editor to a file every update.
2. Alternatively, it can also be started with a flag that tells it to read from a file into the editor every update.
3. FFT: The coder can receive information about current audio levels to drive an effect.

So, we can read the code from a player (aka. the client) using TIC started with feature (1), send that file over the internet to the host (aka. the server), and that code can be channeled into TIC running with feature (2).

We use Ticws to send that code file over the internet. Currently we send to a 'proxy server' in the middle. This listens to a connected client and forwards that data on to a connected server - this allows us to have a static address that both clients and servers can connect to. It also means whoever is running the server doesn't need to open their network for connections.

Note that only the code from the TIC-80 editor gets sent. The code is annotated either with the current cursor position, or with a run-me signal, so that the stream can show you editing and running.

The host will not receive:
- Graphics or music created with the in-built editors
- Mouse or keyboard input while your code is running
- Anything you do on the command line within TIC-80
- Anything you do elsewhere on your computer

## Joining a ByteJam

Your host will provide you with a `room name` and your `username`. You can connect at any time and you'll be connecting to a proxy, so if the host has not yet connected, your updates will disappear into the void. No harm done!

If you are joining on an ARM Mac, go follow [doop's excellent setup instructions](https://gist.github.com/doop/6c0f0783e4b613540cbadb37fc7a2be6). You'll need to build a few things in advance, so allow some fiddle time. (Thanks [doop](https://mastodon.social/@doop@octodon.social)!)

If you're joining on Linux or Windows, you'll need to grab:

- [TIC-80 'alice fork' v0.0.5](https://github.com/aliceisjustplaying/TIC-80/releases)
- [Ticws](https://github.com/totetmatt/ticws)

These need to be in the same folder.

In order to get FFT working, you may need add a couple of flags to the start line in the `run_client.bat` file - this is true on Windows [someone confirm for me on Linux, please? :)]

Change:

```bat
start tic80.exe --skip --codeexport=showdown_%1_%2.dat --delay=5
```

To:

```bat
start tic80.exe --skip --codeexport=showdown_%1_%2.dat --delay=5 --fft --fftcaptureplaybackdevices
```

Then, in a command window, execute

```cmd
run_client.bat room_name username
```

After a few seconds, everything should start up. This will include TIC-80 and Ticws, which will attach to a specific channel on the alkama's proxy server (thanks [Alkama](https://alkama.com/))!

The host will launch an instance of TIC for each player and collect the output of these together into a visual layout (usually decorated with every player's name, and a background). They'll be using some free software [OBS Studio](https://obsproject.com/) to do that... but you only need investigate that if you're curious!

You may be asked to check the stream is working. I usually make a little effect with my username to let the host know which TIC is receiving my code.

I might also test that the FFT audio in TIC is working by starting to type fft( in the code editor... this should turn green as a registered command... and then set up a little fft effect. You can copy this one to try:

```lua
function TIC()
	cls()
	for i=0,135 do
		line(0,i,ffts(i)*239,i,12)
	end
end
```
(Save something like this as `fft-test.tic` for next time?)

Maybe bang on some [h0ffman](https://soundcloud.com/h0ffman) to check you get the visual feedback?

![Some bouncing FFT levels](/image/fft-levels.gif)

If everything has gone well then you'll be set - the stream will launch on Twitch and you can Jam. Note that the stream will be running slightly behind, so there may be 10 or 20 seconds lag between changes and updates on the stream. This is disconcerting but you'll get used to it.

You may also find occasional issues. Some of the time, I'll run code and it'll show an error on the server. Re-running fixes it.

Virtual Bob Ross wishes you many happy little accidents!

