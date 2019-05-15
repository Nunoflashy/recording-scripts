# recording-scripts

Video/Audio Recording Scripts

The general idea was to make it function somewhat similarly to Nvidia Shadowplay,
but I also made some modifications that I thought were necessary for my personal use, 
such as recording audio and video separately.

## WARNING
This script detects recording activity based on ffmpeg, and as such it terminates all ffmpeg processes
when you want to stop recording. If you use ffmpeg for anything else, such as video encoding and you stop this script,
you may get corrupted video!

Once I find a way to rectify this issue, I will update the script, but for now you have been warned!

Alright, with that out of the way, let us continue with the fun stuff.

As it is it's nothing more than a script to start and stop recording,
in order for it to behave like Shadowplay, we need to add some keybindings.

Since I'm using i3, I'll show
you how to configure it for that, but other window managers shouldn't be too hard to configure either.

In my i3 config I have it set like this:

```bash

# RECORDING
bindsym Mod1+F9 exec --no-startup-id record -all
```

So now we got the recording script bound to the same default hotkey as Shadowplay. We should be pretty much done, right?

Not quite, now we need some kind of feedback to know whether or not we are actually recording.

Now we will have to integrate the script with some kind of system monitor or notification daemon.
For this particular script, I use i3bar with i3blocks, however I may add notifications for it in the future as well.

Here's the blocklet in my i3blocks config:

```
[record]
command=cat /tmp/video-recording.record
align=center
color=#F44242
separator=false
separator_block_width=25
signal=2
```

With that done, we are ready to display whether or not we are recording by pressing Alt+F9 (or your preferred hotkey if
you changed it.)


And here's the finished product.

Idle: 
![idle_bar](https://user-images.githubusercontent.com/25163730/57745841-a0823200-76c6-11e9-8a21-df5c8989f6f7.jpg)

Recording: 
![recording_bar](https://user-images.githubusercontent.com/25163730/57745738-36698d00-76c6-11e9-8dcf-712f93646fce.jpg)

