---
layout: post
title: 'Contiki-CC2538-CoAP'
date: 2016-11-17 21:04:40
comments: true
tags:
  - Contiki
  - CC2538
  - CoAP
---

<div align = "center">
<iframe height=498 width=510 src='http://player.youku.com/embed/XMTgyNDA3NDAzNg==' frameborder=0 'allowfullscreen'></iframe>
</div>

<!--more-->

# Mote1

## build rp-border-router

cd ~
cd contiki/examples/ipv6/rpl-border-router

```
user@instant-contiki:~/contiki/examples/ipv6/rpl-border-router$ git diff .
diff --git a/examples/ipv6/rpl-border-router/border-router.c b/examples/ipv6/rpl
index 90b1fdf..f7d565c 100644
--- a/examples/ipv6/rpl-border-router/border-router.c
+++ b/examples/ipv6/rpl-border-router/border-router.c
@@ -54,7 +54,8 @@
 #include <string.h>
 #include <ctype.h>
-#define DEBUG DEBUG_NONE
+//#define DEBUG DEBUG_NONE
+#define DEBUG DEBUG_PRINT
 #include "net/ip/uip-debug.h"
 static uip_ipaddr_t prefix;
diff --git a/examples/ipv6/rpl-border-router/project-conf.h b/examples/ipv6/rpl-
index ec3eb3f..1f4f8c9 100644
--- a/examples/ipv6/rpl-border-router/project-conf.h
+++ b/examples/ipv6/rpl-border-router/project-conf.h
@@ -64,4 +64,7 @@
 #define WEBSERVER_CONF_CFS_CONNS 2
 #endif
+//#define RF_CORE_CONF_CHANNEL    25
+#define RF_CORE_CONF_CHANNEL    26
+
 #endif /* PROJECT_ROUTER_CONF_H_ */
```

`make TARGET=cc2538dk all`

## program cc2538,(select + reset on CC2538DK)

`../../../tools/cc2538-bsl/cc2538-bsl.py -v -e -w border-router.bin`

## push reset button, see console print

`../../../tools/sky/serialdump-linux -b115200 /dev/ttyUSB0`

# Mote2

## build er-rest-example

`cd ~`
`cd contiki/examples/er-rest-example`

```
user@instant-contiki:~/contiki/examples/er-rest-example$ git diff .
diff --git a/examples/er-rest-example/project-conf.h b/examples/er-rest-example/project-conf.h
index adcb75e..facd2fb 100644
--- a/examples/er-rest-example/project-conf.h
+++ b/examples/er-rest-example/project-conf.h
@@ -40,25 +40,26 @@
 #define __PROJECT_ERBIUM_CONF_H__

 /* Custom channel and PAN ID configuration for your project. */
-/*
+
    #undef RF_CHANNEL
    #define RF_CHANNEL                     26

    #undef IEEE802154_CONF_PANID
    #define IEEE802154_CONF_PANID          0xABCD
- */
+
 /* IP buffer size must match all other hops, in particular the border router. */
-/*
+
    #undef UIP_CONF_BUFFER_SIZE
    #define UIP_CONF_BUFFER_SIZE           256
- */
+
 /* Disabling RDC and CSMA for demo purposes. Core updates often
    require more memory. */
 /* For projects, optimize memory and enable RDC and CSMA again. */
 #undef NETSTACK_CONF_RDC
-#define NETSTACK_CONF_RDC              nullrdc_driver
+//#define NETSTACK_CONF_RDC              nullrdc_driver
+#define NETSTACK_CONF_RDC              contikimac_driver
 #undef RPL_CONF_MAX_DAG_PER_INSTANCE
 #define RPL_CONF_MAX_DAG_PER_INSTANCE     1
@@ -68,7 +69,8 @@
 #define UIP_CONF_TCP                   0
 #undef NETSTACK_CONF_MAC
-#define NETSTACK_CONF_MAC     nullmac_driver
+//#define NETSTACK_CONF_MAC     nullmac_driver
+#define NETSTACK_CONF_MAC     csma_driver
 /* Increase rpl-border-router IP-buffer when using more than 64. */
 #undef REST_MAX_CHUNK_SIZE
```

`make TARGET=cc2538dk all`

## program cc2538,(select + reset on CC2538DK)

`../../../tools/cc2538-bsl/cc2538-bsl.py -p /dev/ttyUSB1 -v -e -w er-example-server.bin`

## push reset button, see console print

`../../../tools/sky/serialdump-linux -b115200 /dev/ttyUSB1`

# run tunslip6

`cd ~`
`cd contiki/examples/ipv6/rpl-border-router`
`sudo ../../../tools/tunslip6 aaaa::1/64 -s /dev/ttyUSB0`

```
********SLIP started on ``/dev/ttyUSB0''
opened tun device ``/dev/tun0''
ifconfig tun0 inet `hostname` mtu 1500 up
ifconfig tun0 add aaaa::1/64
ifconfig tun0 add fe80::0:0:0:1/64
ifconfig tun0
tun0      Link encap:UNSPEC  HWaddr 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00
          inet addr:127.0.1.1  P-t-P:127.0.1.1  Mask:255.255.255.255
          inet6 addr: fe80::1/64 Scope:Link
          inet6 addr: aaaa::1/64 Scope:Global
          UP POINTOPOINT RUNNING NOARP MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:500
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

*** Address:aaaa::1 => aaaa:0000:0000:0000
Got configuration message of type P
Setting prefix aaaa::
created a new RPL dag
Server IPv6 addresses:
 aaaa::212:4b00:5af:8444
 fe80::212:4b00:5af:8444
```

# ping6

`ping6 aaaa::212:4b00:5af:84e0`

```
PING aaaa::212:4b00:5af:84e0(aaaa::212:4b00:5af:84e0) 56 data bytes
64 bytes from aaaa::212:4b00:5af:84e0: icmp_seq=1 ttl=63 time=1013 ms
64 bytes from aaaa::212:4b00:5af:84e0: icmp_seq=2 ttl=63 time=126 ms
64 bytes from aaaa::212:4b00:5af:84e0: icmp_seq=3 ttl=63 time=126 ms
64 bytes from aaaa::212:4b00:5af:84e0: icmp_seq=4 ttl=63 time=126 ms
64 bytes from aaaa::212:4b00:5af:84e0: icmp_seq=5 ttl=63 time=125 ms
64 bytes from aaaa::212:4b00:5af:84e0: icmp_seq=6 ttl=63 time=120 ms
```

# firefox

![](/img/20161117/coap1.png)
![](/img/20161117/coap2.png)
