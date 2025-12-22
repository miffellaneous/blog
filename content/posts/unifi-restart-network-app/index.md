+++
title = "Restarting the Unifi Network App without downtime"
date = "2025-11-26T23:22:00Z"
author = "Miffy"
authorTwitter = "" #do not include @
cover = "cover.png"
coverCaption = "My dream machine is having a nightmare."
coverCreditUrl = "https://albys.space/about/"
coverCreditName = "Alby"
tags = ["tech", "unifi"]
#keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
summary = "I somehow upset the Unifi Network Application console interface but managed to restart it without losing my uptime."
+++

Earlier today I was nosing around my Unifi console to see if there were any cool new features to play with, when suddenly the interface became unresponsive; shortly thereafter any attempt to access the Network Application led to an infinite loading bar. My internet connection was fine, and the cute little screen on my Dream Machine was able to report network activity just as the Network Application dashboard would, if it could load.

Unexpectedly, I could navigate my browser to the Protect Application: the camera feeds were live and I could see the disk recordings were uninterrupted as I could scrub backwards in time. All the Protect functionality was operational, but any attempts to navigate back to the Network Application simply hung.

Of course, a reasonable bun would power cycle the box and be done with it.
But I am not a reasonable bun.
The network itself was up and the other Unifi applications were running -- in SLA terms, nothing was down, so why should I incur downtime just to get my pretty dashboard back?

After a little pouting, I remembered I'd set up [SSH on my Unifi Gateway](https://help.ui.com/hc/en-us/articles/204909374-Connecting-to-UniFi-with-Debug-Tools-SSH) and let myself in.
I'd expected to find some stuck process hogging a load of resource, but the box was operating nominally -- no thrasing processes in `htop`, no memory hogs and a low load average.
I saw an `nginx` process running and figured "oh web stuff" and restarted it, to no effect.

I wasn't quite sure where to go from here.
Some spurious articles I found suggested fiddling with `unifi-os` or `podman`, neither of which are installed on my Dream Machine.
There were many programs in the PATH that started with `unifi-` but I was not tempted to mess with `unifi-network-service-common` or `unifi-network-service-helper` without knowing what they did.
I used `systemctl status` to get a tree of the running services in the hope that we might narrow the search:

```
...
 ├─app.slice
 │ ├─app-unifi.slice
 │ │ ├─unifi.service
*│ │ │ └─/usr/bin/java ... -jar /usr/lib/unifi/lib/ace.jar start
 │ │ ├─unifi-mongodb.service
 │ │ ├─mcagent.service
 │ │ └─rabbitmq-server.service
 │ └─app-protect.slice
 │   ├─postgresql@14-protect.service
 │   ├─msp.service
 │   ├─msr.service
 │   ├─ms.service
 │   ├─unifi-protect.service
 │   └─mst.service
...
```

Neatly, the `app.slice` appeared to correspond to the two Unifi Applications I have installed: Network (app-unifi) and Protect (app-protect).
The Network Application is tightly coupled to the workings of the gateway, so it's not unexpected that the `app-unifi` slice simply runs a service named `unifi`, but you'd be forgiven for first thinking that restarting this service could bring down the whole gateway.

A quick Google suggested the `ace.jar`[^ace] (starred in my output above) is indeed the brains behind the network application controller, so I figured this was the best thing to kick and if I was wrong, I'd just reboot the machine.

## tl;dr

The Network Application is controlled by the `unifi` service, so I restarted it.

```
systemctl restart unifi
```

It took around a minute or so for control to return to my shell prompt, presumably there is a lot to do in stopping and starting this service.
During this time the Network Application in my browser appeared as unavailable, but without impact to my internet connection and Protect.
Around a minute after that, the Network Application including its dashboard and other views were all reachable once more.

Success: I restarted the Unifi Network Application without restarting my network. We didn't lose a drop of uptime.

[^ace]: "ACE" is apparently a holdover from "AirControl Enterprise", a precursor of the Unifi Network Application; according to a [guy on the internet](https://community.ui.com/questions/Companion-or-Extension-to-the-UniFi-API/b3e0b7d1-1ab5-411d-aaaf-18fce517f504#answer/85a41206-bb1e-4421-8279-407f5f4f8fdc)
