+++
title = "TYSK: Going nowhere fast with 'Quick access' pins on Windows"
date = "2026-03-07T18:30:00Z"
author = "Miffy"
authorTwitter = "" #do not include @
cover = "cover.png"
coverCaption = "Where we're going, we actually do need files."
coverCreditUrl = "https://albys.space/about/"
coverCreditName = "Alby"
tags = ["tech", "tysk"]
#keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
summary = "I had a quick access pin pointing to nothing and I could not access quick. I removed the pin and quick access was quick once more."
+++

At some point in the recent past I noticed my Windows 10 desktop would hang when:

- Creating a new folder in any Windows explorer window, taking several seconds for the "New folder" to show up
- Opening or closing a system file selector dialog, taking several seconds for control to return to the application that initiated the file selector dialog

This is just one of many mildly annoying things my Windows desktop now does and I had simply attributed the problem to the rapid disassembly of the Windows desktop experience.
More annoying still is that any attempt to fix anything on a Windows desktop these days involves sifting through unhinged suggestions from "independent advisors" on Microsoft forum posts and bewildering YouTube videos that seem to solve every problem by destroying keys in the Windows Registry.

In this "I think you should know", I wanted to share that I managed to fix this problem by clearing out junk pins that I had accumulated in my "Quick access" sidebar in Windows explorer.
Specifically, any pin that points to a network share location that no longer exists.

Turns out that I still had a pin pointing to the {{< unsafe >}}<abbr title="Universal Naming Convention">UNC</abbr>{{< /unsafe >}} of a ZFS pool on a device that I had decommissioned a little while ago.
This innocuous dead pin was the cause of the hangs in Windows explorer when opening file dialogues or creating new folders.

Naturally, everything is obvious in hindsight, and now that I know that this was related to broken network shares, I was even able to find a [Microsoft Knowledge Base article](https://learn.microsoft.com/en-us/troubleshoot/microsoft-365-apps/office-suite-issues/office-stops-responding) about it. Hopefully the search gods will be in your favour and you might have found your way here instead. Let me know if this helped!
