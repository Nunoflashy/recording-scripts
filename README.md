# recording-scripts

Video/Audio Recording Scripts

The general idea was to make it function somewhat similarly to Nvidia Shadowplay,
but I also made some modifications that I thought were necessary for my personal use, 
such as recording audio and video separately.

Of course, as they are it's nothing more than a script to start and stop recording,
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
![normal](https://user-images.githubusercontent.com/25163730/57560176-01370500-737d-11e9-96e6-39c79481c99f.jpg)

Recording Audio: 
![audio](https://user-images.githubusercontent.com/25163730/57560192-1f046a00-737d-11e9-84ef-b995f9039d5c.jpg)

Recording: 
![recording](https://user-images.githubusercontent.com/25163730/57560204-304d7680-737d-11e9-96cc-a99ed2395dc7.jpg)
