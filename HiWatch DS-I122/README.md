# Install OpenIPC on HiWatch DS-I122 (with firmware Rostelecom (Ростелеком))

HyperTerminal

For break boot press Ctrl+U

```
loadb
```

Transfer->Send File "u-boot-hi3518cv100-universal.bin" Kermit

```
go 0x81000000
```

For break boot press any key

```
setenv phyaddru 3
setenv phyaddrd 1
```

***save backup

```
setenv ipaddr 192.168.1.10
setenv serverip 192.168.1.128
mw.b 0x82000000 0xff 0x1000000
sf probe 0
sf read 0x82000000 0x0 0x1000000
tftp 0x82000000 backup-hi3518cv100-nor16m-4447cc9c94a6.bin 0x1000000
```

***burn

```
setenv ipaddr 192.168.1.10
setenv serverip 192.168.1.128
saveenv
mw.b 0x82000000 0xff 0x1000000
tftpboot 0x82000000 openipc-hi3518cv100-lite-16mb.bin
sf probe 0
sf erase 0x0 0x1000000
sf write 0x82000000 0x0 0x1000000
reset
```

For break boot press any key

```
run setnor16m
```

For break boot press any key

```
setenv sensor ar0130
setenv ethaddr 44:47:cc:9c:94:a6
setenv phyaddru 3
setenv extras hieth.phyaddru=3 hieth.mdioifu=0
saveenv
reset
```

http://ip-from-dhcp:85/

username: root

pasword: 12345

***upgrade (wait reboot)

***do reset to default

http://ip-from-dhcp/

set password

***Enable IR. ssh
```
cli -s .nightMode.enabled true
cli -s .nightMode.irCutPin1 6
cli -s .nightMode.irCutPin2 5
cli -s .nightMode.backlightPin 42

curl -o /usr/sbin/wl_solalex.sh https://raw.githubusercontent.com/OpenIPC/sandbox/main/scripts/backlight-control/wl_solalex.sh
chmod +x /usr/sbin/wl_solalex.sh
curl -o /etc/init.d/S99rc.local https://raw.githubusercontent.com/OpenIPC/sandbox/main/scripts/backlight-control/S99rc.local
chmod +x /etc/init.d/S99rc.local
```

Enable onvif, set onvif password.

Set timezone

rtsp://root:password@10.168.110.50:554/stream=0

SRC:

https://mixatronik.ru/videonablyudenie/openipc/zapusk-openipc-na-kamere-hiwatch-ds-i122

https://openipc.org/ru/get-started

https://github.com/OpenIPC/sandbox/tree/main/scripts/backlight-control
