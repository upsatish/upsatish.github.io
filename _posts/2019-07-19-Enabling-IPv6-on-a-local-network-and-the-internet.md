---
layout: post
title:  "Enabling IPv6 on a local network and the internet"
date:   2019-07-19 10:00:00 +0800
categories: update
tags: update ipv6 tech
---
After reading about IPv6, I decided to learn a few things about it by enabling it on my local network and the internet. Watch the first part of [this video](https://www.youtube.com/watch?v=GE_FqZ-XLR0) to learn some IPv6 basics.

## Enabling IPv6 on a local network
The modem-router / gateway on a local network does IP addresses and must support IPv6. On my router’s (Technicolor TG789vac v2) admin interface, I had to switch on an IPv6 state toggle under the Local Network tile to enable IPv6. DHCPv6 will assign IPv6 addresses for your connected devices. The router can work in dual stack network support, running both IPv4 and IPv6 addresses and connecting to IPv4 and IPv6 resources simultaneously.

![IPv6 toggle local network](/assets/img/IPv6 Local Network toggle.png)
![IPv6 assigned local network](/assets/img/IPv6 Assigned Local Network.png)

### Testing IPv6 on a local network
On a workstation terminal your IP address can be viewed with the ifconfig or ip addr (Ubuntu Linux) or ipconfig (Windows) command, depending on your operating system. The output will have an IPv4 and an IPv6 address, allowing your device to communicate with nodes that support both IPv4 and IPv6 addresses.

    $ ifconfig
    ...
    en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
            ether 10:40:f3:88:cf:d2 
            inet6 fe80::14b0:4170:dc02:976f%en0 prefixlen 64 secured scopeid 0x6 
            inet6 2403:5101:c600:2800:18eb:6d00:d4e6:667c prefixlen 64 autoconf secured 
            inet6 2403:5101:c600:2800:25a2:67ac:d9e7:6b90 prefixlen 64 autoconf temporary 
            inet 192.168.1.180 netmask 0xffffff00 broadcast 192.168.1.255
            nd6 options=201<PERFORMNUD,DAD>
            media: autoselect
            status: active
    ...

On my phone and workstation, the IPv6 addresses can be viewed in network settings.

The router’s IPv6 address can be viewed using netstat - default gateway.

    $ netstat -nr|grep ^default
    default            192.168.1.1        UGSc           60        0     en0
    default                                 fe80::a651:b188:ecff:fe91%en0   UGc             en0
    default                                 fe80::%utun0                    UGcI          utun0
    ...

Try ping the router’s address using ping6.

    $ ping6 -c 4 fe80::a651:b188:ecff:fe91%en0
    PING6(56=40+8+8 bytes) fe80::10cb:83cd:e102:863%en0 --> fe80::a651:b188:ecff:fe91%en0
    16 bytes from fe80::a651:b188:ecff:fe91%en0, icmp_seq=0 hlim=64 time=2.149 ms
    16 bytes from fe80::a651:b188:ecff:fe91%en0, icmp_seq=1 hlim=64 time=2.855 ms
    16 bytes from fe80::a651:b188:ecff:fe91%en0, icmp_seq=2 hlim=64 time=2.232 ms
    16 bytes from fe80::a651:b188:ecff:fe91%en0, icmp_seq=3 hlim=64 time=1.656 ms
    
    --- fe80::a651:b188:ecff:fe91%en0 ping6 statistics ---
    4 packets transmitted, 4 packets received, 0.0% packet loss
    round-trip min/avg/max/std-dev = 1.656/2.223/2.855/0.426 ms

Try http://[fe80::a651:b188:ecff:fe91%en0] to go to the router’s admin interface. Unfortunately it didn’t work for me, possibly because the web server hosting the admin interface does not have an IPv6 binding set up. I tried a few more things, to see if there was an alternative binding, but no.

    $ nslookup 192.168.1.1
    Server:		2403:5101:c600:2800::1
    Address:	2403:5101:c600:2800::1#53
    
    1.1.168.192.in-addr.arpa	name = dsldevice.lan.
    
    $ ping6 dsldevice.lan
    ping6: getaddrinfo -- nodename nor servname provided, or not known

## Enabling Ipv6 for Internet
To enable IPv6 for the internet, your ISP has to support IPv6. Or you could use an IPv6 tunnel broker. I joint Aussie Broadband’s IPv6 beta via [https://my.aussiebroadband.com.au/ipv6beta.php](https://my.aussiebroadband.com.au/ipv6beta.php). The router must support IPv6 and IPv6 must be enabled, as I did in the Local Network section.

My router picked this up automatically. If you have to configure anything at all with your ISP it would be DNS.

![IPv6 connected internet](/assets/img/IPv6 Connected Internet.png)

### Testing IPv6 for Internet
I tested my IPv6 address using [Google what is my IP](https://www.google.com/search?q=what+is+my+ip)

![Google what is my IP](/assets/img/Google what is my IP.png)

I also used the [IPvFoo chrome extension](https://chrome.google.com/webstore/detail/ipvfoo/ecanpcehffngcegjmadlcijfolapggal?hl=en) to see if the current page was fetched using IPv6 or IPv4. [https://github.com/pmarks-net/ipvfoo](https://github.com/pmarks-net/ipvfoo)

On https://www.google.com, all resources are fetched using IPv6, except https://ssl.gstatic.com, which is cached.

![IPvFoo 6 4](/assets/img/IPvFoo.png)

![IPvFoo cached results](/assets/img/IPvFoo cached results.png)

I disabled the cache in the Network tab of Chrome Developer Tools and all resources are fetched using IPv6 now.

![IPvFoo OK](/assets/img/IPvFoo OK.png)

![IPvFoo OK results](/assets/img/IPvFoo OK results.png)

Other websites to test with:

[IPv6 test - IPv6/4 connectivity and speed test](https://ipv6-test.com/)

[IPv6 Test and Dual-Stack Test For Network Connectivity](http://testmyipv6.com/)

[Test your IPv6.](https://test-ipv6.com/)

## See also
[IPv6 - Wikipedia](https://en.wikipedia.org/wiki/IPv6)

[IPv6 \| Aussie Broadband help centre](https://www.aussiebroadband.com.au/help-centre/nbn/tech-support/ipv6/)

[You have IPv6. Turn it on. \| APNIC Blog](https://blog.apnic.net/2016/05/04/you-have-ipv6-turn-it-on/)

[IPv6 test - Statistics for Australia](https://ipv6-test.com/stats/country/AU)

[IPv6 Support whirlpool wiki](https://whirlpool.net.au/wiki/hw_feature_242)

