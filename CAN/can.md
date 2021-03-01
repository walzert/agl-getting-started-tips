# CAN-Bus on AGL

CAN-bus can be used with the **[agl-service-can-low-level][1]** .

Therefore a CAN bus device must be attached e.g. a virtual (vcan) or a serial (slcan).

To enable a virtual can device use following commands

``` 
modprobe vcan
ip link add vcan0 type vcan
ip link set vcan0 up
```
To enable a serial can  (slcan)  device use following commands

``` 
/sbin/modprobe slcan; 
/usr/bin/slcan_attach -f -s6 -o /dev/ttyACM0;
/usr/bin/slcand ttyACM0 slcan0;
/sbin/ifconfig slcan0 up
```

``` 
modprobe slcan; 
slcan_attach -f -s6 -o /dev/ttyACM0;
slcand ttyACM0 slcan0;
ifconfig slcan0 up
```

that could although be done with a service-file in /lib/systemd/system my [example](socketcan-interface-slcan0.service) for the [USBtin](https://www.fischl.de/usbtin/) slcan-adaper.


After that you are able to use the commands 

```
candump CANDEVICE(vcan0 | slcan0) // print the messages on canbus
cangen CANDEVICE(vcan0 | slcan0)  // generate random messages on canbus
cansend CANDEVICE(vcan0 | slcan0) 123#DEADBEEF // send a specific message with id#bytes

```
For the right mapping for the agl-service-can-low-level you need a file /etc/dev-mapping.conf with :

```
; Default CAN device mapping
; Format has to follow ini rules key="value", notice " around value.

[CANbus-mapping]
hs="slcan0"
ls="slcan0"
```

according with your candevice. After that `chsmack -a '_' /etc/dev-mapping.conf` and do a `rebooot` .

By entering the following command attention for the right version, you could check that with `afm-util list -a `

```
afm-util detail agl-service-can-low-level
```

you should get following information and we are interrested in the **http-port** : 

```

{
  "description":"Expose CAN Low Level APIs through AGL Framework",
  "name":"agl-service-can-low-level",
  "shortname":"",
  "id":"agl-service-can-low-level@8.0",
  "version":"8.0.0-3977705",
  "author":"Romain Forlot <romain.forlot@iot.bzh>",
  "author-email":"",
  "width":"",
  "height":"",
  "icon":"/var/local/lib/afm/applications/agl-service-can-low-level/8.0/icon.png",
  "http-port":31022
}

```

After that you should be able with `afb-client-demo --human 'localhost:31022/api?token=HELLO&uuid=magic' ` to connect to the serice.

There you could use the following commands [AGL Docs CAN][2] :

```
low-can list // to get a list of all messages
low-can subscribe {"event": "*"} // subscribe to all messages
low-can auth  // authorize writing
low-can write { "signal_name": "hvac.temperature.left", "signal_value": 2} //send can message 
low-can write { "signal_name": "hvac.temperature.average", "signal_value": 2} // send can message

```

The upper sequence should give you json messages back.


[1]: https://git.automotivelinux.org/apps/agl-service-can-low-level/about/
[2]: https://docs.automotivelinux.org/docs/en/master/apis_services/reference/signaling/5-Usage.html


## Cannelloni

cannelloni -I can0 -R "IP-AGL" -r 20000 -l 20000

cannelloni -I vcan0 -R "IP-PC" -r 20000 -l 20000
