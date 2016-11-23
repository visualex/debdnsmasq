# debdnsmasq
network wide ad-blocking on debian / ubuntu 

this also works well on a pi in a small (home) network.

### install dnsmasq

```
$ apt-get install dnsmasq

```

* mv the default /etc/dnsmasq.conf to /etc/dnsmasq.conf.backup
* then vi /etc/dnsmasq.conf 
* add this: 

```
domain=raspberry.local
resolv-file=/etc/resolv.dnsmasq
min-port=4096

# use google dns
server=8.8.8.8
server=8.8.4.4

# (optional) additional hosts file
# useful for adding network wide local domains or 
# blocking domains which are not listed in /etc/dnsmasq.block.conf
addn-hosts=/etc/hosts2

# ad servers list
conf-file=/etc/dnsmasq.block.conf

# max cache size for dnsmasq
cache-size=10000

```

* add the file to sysupgrades

```
echo "/etc/hosts2" >> /etc/sysupgrade.conf
echo "/etc/dnsmasq.block.conf" >> /etc/sysupgrade.conf

```

* add the following as a bash script and run as root cronjob (every week, at teatime)

```
#!/bin/bash

curl -s "http://pgl.yoyo.org/as/serverlist.php?hostformat=dnsmasq-server&showintro=0&startdate%5Bday%5D=&startdate%5Bmonth%5D=&startdate%5Byear%5D=&mimetype=plaintext" >> /etc/dnsmasq.block.conf

/etc/init.d/dnsmasq restart

```

setup this server with a static IP and point your routers DNS servers to this IP.
reconnect to the network on your devices, you should not see any more ads now. 
