+++
title = "Removing ghost clients from the Unifi Network App"
date = "2025-12-22T12:17:00Z"
author = "Miffy"
authorTwitter = "" #do not include @
cover = "cover.png"
coverCaption = "Kicking the ghosts out of my dream machine."
coverCreditUrl = "https://albys.space/about/"
coverCreditName = "Alby"
tags = ["tech", "unifi"]
#keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
summary = "If there's something strange in your dream machine, who you gonna call? MongoDB!"
+++

A few months ago I added a PCIe network adapter to one of my older servers that had earned the right to talk faster than 1 Gb, but was not quite deserving of SFP+.
In doing so, I patched the server into my shiny 2.5 Gb switch, an upgrade from its previous port in my crowded 1 Gb internet-of-shit switch.

This afternoon while nosing around the Unifi Network App as I occasionally do to poke at settings and gaze at graphs, I noticed that the "Ports" interface for the 1 Gb switch showed a "ghost" of the server on its previous port.
Both the cute colour coded Port Diagram and the Connection column in the Port Table featured a greyed out reference to this server, despite the interface indicating the server was "last seen" on that port months ago.

I remembered noticing this discrepancy when I'd moved the server from one switch to the other, but had figured that the interface would age out this connection after some short period of time.
Unfortunately, I suspect that this is only the case for disconnected clients: *i.e.* a device with a MAC address that has left the network and not returned.
I like my computers to be in a state of general correctness, so I was naturally irrationally annoyed by this and set about to resolve the inaccuracy, immediately.


## Finding ghosts

I was aware that much of the Unifi Network App interface is populated by information stored in a Mongo database on the gateway, so had a feeling I could set this right there.
I [SSH'd to my Unifi Gateway](https://help.ui.com/hc/en-us/articles/204909374-Connecting-to-UniFi-with-Debug-Tools-SSH) and connected to the database:

```
mongo --port 27117
```

A little bit of `show dbs`, `show tables` and empty `db.collection.find`'s later, I had a query to interrogate the `device` collection[^ace] which records a `last_connection` object for each of the devices ports in a `port_table` field, and find those that have a disconnected "ghost".

```
use ace
db.device.aggregate([
  { $unwind: "$port_table" },
  { $match: { "port_table.last_connection.disconnected_at": { $exists: true } } },
  { $project: { _id: 0, name: 1, hostname: 1, model: 1, port_table: 1 } }
])
```

I confirmed that these results corresponded to ghosts in the Unifi Network App.
Looking at unassigned ports it looks as though we'd want to `unset` the `last_connection` field for our ghosts.
I'd also need to inspect the disconnected timestamp to not overzealously remove devices that were taking a nap, like my work laptop now that I'm off for the holidays.

## Busting ghosts

I'd planned to use an `updateMany` to do this, but was a little spooked by the syntax.
Wanting to avoid blasting away all my connection information, I cowardly opted to simply:
- Iterate devices with a `port_table`
- Iterate ports in the each device `port_table`
- Reset the `last_connection` field `if` the last seen timestamp is old

I'd be able to comment out the update to see what records would be affected before doing anything.
Here's one I made earlier:

```
use ace
const last_seen_cutoff = Math.floor(Date.now() / 1000) - (60 * 60 * 24 * 30)  // 30 days
db.device.find({ port_table: { $exists: true } }).forEach(doc => {
    doc.port_table.forEach((port, idx) => {
        if (
            port.last_connection &&
            port.last_connection.disconnected_at &&
            port.last_connection.disconnected_at < last_seen_cutoff
        ) {
            printjson({
                _id: doc._id,
                model: doc.model,
                port: port.port_idx,
                last_connection: port.last_connection
            });
            // Uncomment below to execute the unset
            //  I've left this commented to make sure you're not simply pasting
            //  arbitrary code from a bun on the internet into a prod database.
            //db.device.updateOne(
            //    { _id: doc._id },
            //    { $unset: { [`port_table.${idx}.last_connection`]: "" } }
            //);
        }
    });
});
```

After executing this query, I was excited to discover that absolutely nothing had happened.
I wasted a bit of time snooping around the database for other sources of similar information before considering that the application might cache port information to build the interface quickly.
Seeing as we know [we can safely restart the Unifi Network App without impacting the network](https://miffellaneous.net/posts/unifi-restart-network-app/), I figured it was worth a try.

```
systemctl restart unifi
```

After a minute or so, the Unifi Network App was back again, and the ghosts had been busted.
I provide this query in the hope that it will be useful to someone else, this worked on my circa 2024 Dream Machine, but your mileage may vary.


[^ace]: The `device` collection is stored in the `ace` database, [which is a name we have seen before](https://miffellaneous.net/posts/unifi-restart-network-app/#fn:1).
