# recording-scripts

Video/Audio Recording Scripts

The general idea was to make it function somewhat similarly to Nvidia Shadowplay,
but I also made some modifications that I thought were necessary for my personal use, 
such as recording audio and video separately.

Of course, as they are it's nothing more than a script to start and stop recording,
in order for it to behave like Shadowplay, I've added some keybindings. 
Since I'm using i3, I'll show
you how to configure it for that, but other window managers shouldn't be too hard to configure either.

In my i3 config I have it set like this:

```bash

# RECORDING
bindsym --release Mod1+F9 exec --no-startup-id record --all   && exec pkill -RTMIN+2 i3blocks
bindsym --release Mod1+F8 exec --no-startup-id record --audio && exec pkill -RTMIN+3 i3blocks
```

So now that we have the scripts bound to hotkeys, we should be pretty much set, right?

Not quite, now we need some kind of feedback to know whether or not we are actually recording.
If you're familiar with i3 and i3blocks, then you probably already know what those lines do, if not
then I'll explain briefly.

The first part just binds the keys to the script, so we don't have to type it manually in a terminal,
the second part will signal i3blocks each time the keys are pressed. According to that signal we can execute
yet another script to display if we are recording or not.

Now time to integrate our recording scripts into i3blocks!

Blocklets in i3blocks config:

```
[audio_recording]
align=center
color=#4286f4
separator=false
signal=3

[recording]
align=center
color=#F44242
separator=false
signal=2
```

With our blocklets added into the config, now we actually need to make them.
Thankfully it's not that complicated, here's how I have mine set up:

audio_recording
```bash
#!/bin/bash

VAR="$SCRIPTS/recording/.rec"
RECORDING="$(cat $VAR)"

function setup {
	touch $VAR
	echo 1 > $VAR
}

# Toggle the variable so the state changes each time this is executed
# If it's on, it will be off on the next execution, and vice versa.
function toggle {
	if [[ $RECORDING = 0 ]]; then
		echo 1 > $VAR
	elif [[ $RECORDING = 1 ]]; then
		echo 0 > $VAR
	fi
}

if [[ ! -f $VAR ]]; then
	setup
fi

toggle

if [[ "$RECORDING" = 0 ]]; then
	# Recording should begin here
	printf "⏺ AUDIO"
fi
```

and here's the one for video:

recording
```bash
#!/bin/bash

VAR="$SCRIPTS/recording/.rec"
RECORDING="$(cat $VAR)"

function setup {
	touch $VAR
	echo 1 > $VAR
}

# Toggle the variable so the state changes each time this is executed
# If it's on, it will be off on the next execution, and vice versa.
function toggle {
	if [[ $RECORDING = 0 ]]; then
		echo 1 > $VAR
	elif [[ $RECORDING = 1 ]]; then
		echo 0 > $VAR
	fi
}

if [[ ! -f $VAR ]]; then
	setup
fi

toggle

if [[ "$RECORDING" = 0 ]]; then
	# Recording should begin here
	printf "⏺ REC"
fi

```

Now admittedly it's not the most elegant solution, I could probably use only one script and pass an argument for audio and another for video, which I might do in the future.
Another change I would like to make is to avoid using a temporary file as a variable, but I haven't found a solution for that yet.

And here's the finished product.

Idle: 
![normal](https://user-images.githubusercontent.com/25163730/57560176-01370500-737d-11e9-96e6-39c79481c99f.jpg)

Recording Audio: 
![audio](https://user-images.githubusercontent.com/25163730/57560192-1f046a00-737d-11e9-84ef-b995f9039d5c.jpg)

Recording: 
![recording](https://user-images.githubusercontent.com/25163730/57560204-304d7680-737d-11e9-96cc-a99ed2395dc7.jpg)
