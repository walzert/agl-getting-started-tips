# Display 

If you want to rotated the display you could do that in the file `/etc/xdg/weston/weston.ini`



```
[output]
name=eDP-1
transform=270
```

you can find the name of your display in `/run/platform/display/weston.log`


```
[13:31:25.343] DRM: head 'eDP-1' found, connector 69 is connected, EDID make 'SEC', model 'unknown', serial 'unknown'
[13:31:25.343] DRM: head 'DP-1' found, connector 76 is disconnected.
[13:31:25.344] DRM: head 'HDMI-A-1' found, connector 84 is disconnected.

```
 