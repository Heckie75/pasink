# pasink
Pulse-audio sink-setter with bluez and combined-sink support

```
Usage:
  pasink [options] [alsa_device] [bluez_device]

Options:
  -h,  --help                             this help text
  -v,  --vol <0-100>                      set volume for default sink (master)
  -va, --vol-alsa <0-100> <alsa_device>   set volume for alsa sink
  -vb, --vol-blue <0-100> <bluez_device>  set volume for bluetooth audio sink
  -l,  --list                             list default, alsa, bluez and combined sinks
  -d,  --dump                             dump more technical information of sinks
  -p,  --pair <mac>                       pair new bluetooth a2dp device

Examples:
  pasink -va 100 -vb 80 belkin hdmi     - creates combined sink of belkin
                                          (an already paired bluetooth device)
                                          and HDMI (alsa card device)
                                          and sets volume for each device
```

## Examples

1. List sinks

```
$ pasink -l

Default sink:
	Aureon Dual USB Digital Stereo (IEC958)

Available Alsa card devices:
	Internes Audio Digital Stereo (HDMI)
	Aureon Dual USB Digital Stereo (IEC958)

Available Bluetooth A2DP devices:
	00:13:17:D0:1E:C0	paired	Nokia AD-42W
	00:02:72:EC:7E:DA	sinked	Belkin C63
  
 ```
 
 2. Connect audio to bluetooth device "Nokia". It is already paired but at this moment sink is setup for bluetooth device "Belkin"
 
 ```
 $ pasink nok

Disconnect:	00:02:72:EC:7E:DA 		[OK]
Connect:	00:13:17:D0:1E:C0 		[OK]
Wait for sink:	bluez_sink.00_13_17_D0_1E_C0 	[OK]

Default sink:	Nokia AD-42W 	[OK]
```

3. Create combined sink with bluetooth and alsa device "hdmi"

```
$ pasink nok hdmi

Create combined sink of:
	Internes Audio Digital Stereo (HDMI)
	Nokia AD-42W

Default sink:	Simultaneous output to Internes Audio Digital Stereo (HDMI), Nokia AD-42W 	[OK]
```

4. Create combined sink with bluetooth device and alsa device "usb"

Volume for alsa device is 100%. Volume for bluetooth audio device is 80%. Master volume (default sink which is combined sink aferwards) is 100%
```
$ pasink -v 100 -va 100 -vb 80 nok usb

Create combined sink of:
	Aureon Dual USB Digital Stereo (IEC958)
	Nokia AD-42W

Default sink:	Simultaneous output to Aureon Dual USB Digital Stereo (IEC958), Nokia AD-42W 	[OK]

Set alsa volume:	100%
Set bluetooth volume:	80%
Set master volume:	100%
```

5. Set master volume 

```
$ pasink -v 80

Set master volume:	80%
```
