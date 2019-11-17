# Network  

If you want to give the device a fixed ip adress you could use following commands:


First use  `connmanctl services` to get the current network connections
 for eg. **ethernet_0007324a8eb2_cable**



```
connmanctl config ethernet_0007324a8eb2_cable --ipv4 manual IPADRESS 255.255.255.0 GATEWAY

route add default gw GATEWAY

```