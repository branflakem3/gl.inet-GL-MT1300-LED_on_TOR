# gl.inet-GL-MT1300-LED_on_TOR
This repo will attempt to provide useful information on how to setup the GL-MT1300 to change the LED when connected to TOR. Others have done it with VPN's but I have not found a repo with anything about the TOR function. 
- [Thanks to ray308 on the gl-inet forums](https://forum.gl-inet.com/t/gl-mt1300-led-control-instructions/13338/22).

1. Rename /usr/bin/mt1300_led_deamon
```
mv /usr/bin/mt1300_led_deamon /usr/bin/mt1300_led_deamon.OLD
```

2. Create replacement mt1300_led_deamon
```
nano /usr/bin/mt1300_led_deamon
```
 
3. Paste this into file
```
#!/bin/sh
ip="1.1.1.1"   # use any dns you want
count="1"
timeout="1"
mt1300_led off
mt1300_led blue_breath daemon

while true; do

status=$(ping -q -c "$count" -W "$timeout" "$ip"  > /dev/null 2>&1 && echo "ok" || echo "fail")

if [ "$status" = "ok" ]; then
	if [ -d /sys/class/net/tun* ]; then    
		mt1300_led white_breath daemon   
	else    
		mt1300_led white daemon      
	fi
else
	mt1300_led blue_breath daemon
fi

sleep 10

done
```

3. Make file executable
```
chmod u+x /usr/bin/mt1300_led_deamon
```

4. Restart LED daemon
```
/etc/init.d/led restart
```
