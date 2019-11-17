# Audio on AGL

You could use aplay (alsaplay) and arecord (alsarecord) to test your audio devices if they are not working out of box.

```
aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: HDMI [HDA Intel HDMI], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: HDMI [HDA Intel HDMI], device 7: HDMI 1 [HDMI 1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: HDMI [HDA Intel HDMI], device 8: HDMI 2 [HDMI 2]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: HDMI [HDA Intel HDMI], device 9: HDMI 3 [HDMI 3]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: HDMI [HDA Intel HDMI], device 10: HDMI 4 [HDMI 4]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: PCH [HDA Intel PCH], device 0: ALC280 Analog [ALC280 Analog]
  Subdevices: 0/1
  Subdevice #0: subdevice #0

```
For example the ALC280 Analog of the system is card **1** and device **0**

`aplay -c 2 --device="plughw:1,0"  /dev/random`

you should here some noise on your speaker.

If that is working you can modify `/etc/wireplumber/wireplumber.conf`
with your numbers

```
load-module C libwireplumber-module-simple-policy {
  "default-playback-device": <"hw:1,0">,
  "default-capture-device": <"hw:1,0">,
  "role-priorities": <{

```

after that `chsmack -a '_' /etc/wireplumber/wireplumber.conf` and `reboot`