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
