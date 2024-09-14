# pasink - Pulse-audio sink-setter with bluez and combined sink support

`pasink` is a command line interface (CLI) which allows easily to switch audio sinks. It support ALSA devices and also Bluetooth A2DP Audio sinks. Sometimes you might want to hear your music on your stereo in your livingroom, which is directly connected to your computer, but also in the kitchen where your have a bluetooth audio device. For this situation pasink allows you to create a combined audio sink. pasink automatically connects your already paired bluetooth device and create a so called 'combined sink'. In case that you have a bluetooth audio device also in your bedroom - like me - you can also switch bluetooth devices just by giving the name of the bluetooth device. 

Take a look at the examples for more details. 

```
Pulse-audio sink-setter with bluez and combined-sink support

Usage:
  pasink [options] [alsa_device] [bluez_device]

Options:
  -h,  --help                             this help text
  -v,  --vol <0-100>                      set volume for default sink (master)
  -va, --vol-alsa <0-100> <alsa_device>   set volume for alsa sink
  -vb, --vol-blue <0-100> <bluez_device>  set volume for bluetooth audio sink
  -l,  --list                             list default, alsa, bluez and combined sinks
  -ll, --list-all                         list more technical information of sinks
  -j,  --json                             simular to list-all but in json format
  -d,  --disconnect                       disconnect all bluetooth devices
  -p,  --pair <mac>                       pair new bluetooth a2dp device

Examples:
  pasink -va 100 -vb 80 belkin hdmi     - creates combined sink of belkin
                                          (an already paired bluetooth device)
                                          and HDMI (alsa card device)
                                          and sets volume for each device
```

## Important note
This script doesn't work anymore on systems using pipewire like Ubuntu 23.04 or later. I have used this script on Ubuntu 22.04.
There is a new script that is exclusivly writter for Pipewire, see [pwsink](https://www.github.com/heckie75/pwsink)

## Requirements / pre-conditions

pasink interally uses the following:

1. `bluez` - Bluetooth tools and daemons, especially the command `bluetoothctl`

The script uses `bluetoothctl` in order to list already paired A2DP bluetooth devides and to connect to them by name or mac address. 

2. `expect`

Internally pasink remote controls `bluetoothctl` like a macro. This will be done by using `expect`
`expect` can be installed as follows:

```
$ sudo apt install expect
```

3. `pulseaudio`

Pulse-Audio is the audio server. It should come with any state-of-the-art linux distribution out of the box. You might want to check if the following command line tools are available which are called by `pasink` internally:

a) `pacmd`

You MUST check and adjust some configuration. Please make sure that the following line is in the file `/etc/pulse/default.pa`:

```
load-module module-switch-on-connect
```

This module is required because it is responsible to automatically setup a new audio sink after a bluetooth A2DP device has been connected.  

4. GNU Awk

Please check if GNU Awk (`gawk`) is installed.

```
$ awk --version
GNU Awk 4.2.1, API: 2.0 (GNU MPFR 4.0.2, GNU MP 6.1.2)
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
