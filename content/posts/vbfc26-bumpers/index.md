+++
title = "VBFC 26: Behind the Bumpers"
date = "2026-04-20T14:00:00Z"
author = "Miffy"
authorTwitter = "" #do not include @
cover = "cover.png"
coverCaption = "This is a mif.fyi service announcement"
coverCreditUrl = "https://theartofmarci.com/"
coverCreditName = "Marci"
tags = ["bfc", "video", "makes"]
#keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
summary = "I made some videos and then I watched them on TV."
+++

Last weekend I attended VBFC for the first time (in fact, this was Miffy's first con, virtual or otherwise).
With seven channels of programming airing over two days, I couldn't believe how much was going on for just $15.
A mailer with a TV guide, stickers and a patch[^patch] coming all the way from the US for no extra charge was a real surprise too.

I won't recap specifics of what I did at the con; I find whenever I attempt to commit subjective experience to the page it will quickly become more of a mission log than a blog post.
Instead I'll just say that having spent two evenings in the company of hundreds of people unashamedly being themselves has left me overcome with joy.

The TV theme was an incredible way to showcase the diverse skills and interests that our community has.
Where else would you be able to watch and learn how to code, cook, draw, woodshop, birdwatch, railroad, ...and poop?
I am just in awe of all the talented and interesting people in our community who are just going around doing cool stuff, and not stopping to go to the bathroom.

I'm so happy to have put myself out there, and receive such a wonderful, welcoming experience in return.
I've plenty of nice first time memories but none as raw as the tears streaming down my face as I heard Rad's voice cracking during a reading of *The Letter* (from *Frog and Toad are Friends*) at the closing ceremony.

But enough tears: one of the unforgettable aspects of VBFC were the community created bumpers (short videos) that aired during panel intermissions.
VBFC aired 112 different bumpers and our little group enjoyed them so much we'd occasionally skip around between channel intermissions to catch as many of them as possible[^bm].

In a previous life I'd made a few videos, so I was excited to have a reason to dust down the tripod and engage in some low-stakes creativity.
I was determined to make at least one bumper but once I'd started the first, I had all sorts of ideas for more.
In the end I made 3 videos which aired as 5 bumpers, and I'd love to tell you a bit about them.

Roll VT!

---

# Everything is awesome

{{< youtube 05bRHixd1PI >}}

I made this one first.
I was thinking of interesting ways to communicate "You are watching VBFC" and remembered seeing [an intro to an episode of Peter Serafinowicz' "Look around You"](https://www.youtube.com/watch?v=t4CRCJUmWsM&t=42s) where a terminal is used to print LOOK AROUND YOU infinitely.
This was useful as it also gave me an excuse to show off my [new keyboard](https://bsky.app/profile/mif.fyi/post/3mj6dk3txjc27).


Hopefully you noticed the monitor looked like Boop[^boop].
I sliced a cardboard box to extract a frame, painted it blue and superglued 3D printed yellow dials.
I found a colourful hat that seemed apt for Boop and hung it off the corner, as it kept falling off the top, which turned out to be better for framing anyway.
Beyond that, there's a few things that you won't get to take in from just 20 seconds.

The terminal that I am interacting with is real, this is actually Windows Powershell [audience: gasp].
Like most modern terminal applications, the appearance settings are overpowered and allowed me to set a picture of Boop's face that I drew as the terminal background, add a margin from the edges, and apply a "retro terminal effect".

{{< figure src="boops_800.jpg" alt="A blue cardboard square is placed around a monitor. The monitor screen background is the face of VBFC Boop. The message of the day shows a fictional BFC RegressOS." position="center" caption="Running the BROADCAST command on RegressOS" captionPosition="center" >}}

In the bumper you see me interacting with "RegressOS".
This is just a Python program masquerading as a shell.
The MoTD declares the version is Jazzy Jackrabbit[^jj] 26.04.1 with a few system messages applicable for the VBFC audience, followed by a shell prompt for the `miffy` user (that's me!).
The shell only allows one "command", `BROADCAST`, which really just means "echo indefinitely".

You may notice the host is set to `boops`.
I'd like to tell you this means Boop's Server or something, but for reasons unknown I'd been calling Boop Boops the whole time by mistake, until the TV guide arrived and cleared things up.

After clearing my entire desk into a doom pile off-camera, my partner generously spent a good chunk of their evening to help me get the shot that I wanted, as it turned out to be impossible to get the composition I wanted of the screen, the keyboard, the blinkenlights of my homelab and my Bab hat, from the position I was sat in.

I used DaVinci Resolve to edit.
I'm really impressed with just how much you can do with the free version of the software, and would recommend it.
I just need to work out how to unmap the mouse scrollwheel from its default configuration of doing anything but what I expected.
DaVinci Resolve even has a sound editing suite built-in that let me normalise the keyboard volume between the wide and close-up shots, filter out the ambient whirring of the homelab and suppress the sound of me breathing that my phone seems very attuned to picking up.
If I were to make more videos, I would certainly be investing in a directional microphone to avoid that.

I'm actually really happy with how it came out, given that I haven't made a video for over ten years.
Not only did I catch a glimpse of it during the opening ceremony montage, it was also the final bumper to play in the main channel before VBFC was brought to a close.


# Pizza

I had the idea for this one after a suggestion that little chefs should make a bumper, and I'll take any excuse to fire up the pizza oven.
I thought it would be fun if this one told a story, so I broke it up into three parts so viewers would get a bit of variety and piece together the pizza's lifecycle over the course of several intermission slots.



First, I make the pizza:
I detach myself from a ball of dough that has slow proofed in the fridge for 24 hours, press and stretch it out, before topping with tomato sauce (Mutti, of course), pepperoni, jalapeño, fresh chilli (from the only plant I have yet to kill) and finally, cheese, cheese, and additional cheese.

{{< youtube iYrxxjuXiL0 >}}

This was pretty fun to edit. It was straightforward to take subclips of the main video to shorten the sequence down, then tidy the transitions from there.
I thought about adding various bopping noises as the toppings got added, but I liked the idea that all three of the bumpers would be calm for viewers and only feature the noises of the birds in our garden.

Second, we watch the pizza bake in the oven.
This was a freebie really, it's just a timelapse shot on my phone.
I'd originally intended only to air up to the first turn (when we rotate the pizza with the peel), but after the initial rise it wasn't as interesting as I hoped, so I sped the entire sequence up to include the whole bake.
I recorded a separate video next to the pizza oven afterwards to get the audio of the gas burning to lay over the silent timelapse.

{{< youtube IBOQC0OCdEI >}}

We then got to enjoy it.
The noises as we slide it off the peel and crunch through it with the cutter is pure pizza ASMR.
You can see the gluten in the dough is nice and strong as the pizza slices are holding their own weight when picked up one by one by their crust.
I would love to have had the forethought to have printed the BFC TV logo onto something under the cut pizza so it could be revealed slice by slice.
If you listen carefully there's a somewhat awkward cut in the audio as I pick up the final slice because a motorbike decided to race down the street at that exact moment.

{{< youtube vxJO9sVCmJM >}}

I really enjoyed that every time any of my pizza bumpers appeared, the chat would erupt into a cacophony of "PIZZA!".


# Suica

{{< youtube ooV3hUgoWgU >}}

This one I'm pleased with, not just because it turned out pretty cute but also because I made the entire thing from action to render in just under an hour[^mtp].
The idea came after chatting about my trip to Japan and my fondness for Suica Penguin, the beloved mascot of the East Japan Railway Company.
I wanted to take the familiar jingle that plays as trains are boarding from some of Tokyo's stations and set it to close-ups of Suica for dramatic effect.
I wanted to include my N guage model of a Yamanote Line train and was really happy with how good the focus pull from the train to Suica looked.

The challenge was to pad the video out as the Yamanote Line jingle I was using wasn't long enough for a bumper, so I found some audio of a train pulling off and mixed that into a shot of me pulling the model away from Suica.

---

# Bubbles the Bun PNGTuber

For those of you who don't know, Bubbles was a toy clown who is iconic for appearing in a [broadcasting test card that was aired in the UK for decades](https://en.wikipedia.org/wiki/Test_Card_F).
I thought it would be on point to have Miffy wearing makeup and clothes to look like this broadcasting icon, given the theme of BFC TV.
A huge thanks to [Alby](https://albys.space/about/) for hooking me up with a PNGTuber at pretty short notice so I could represent myself beyond a static avatar at VBFC.
I really had a great time playing with this PNGTuber by applying ridiculous transforms in OBS Studio[^rotate].

{{< figure src="bubbles_800.jpg" alt="Miffy is at the centre of a pink-purple broadcast test card. The mif.fyi sticker is at the bottom left and the BFC TV 2026 sticker is at the bottom right. Miffy is wearing makeup and clothes to look like Bubbles the Clown: a toy clown that featured on UK broadcasting test cards for decades." position="center" caption="Miffy as 'Bubbles the Bun', ready for their close up on a test card" captionPosition="center" >}}


---

Thanks for watching!


[^patch]: I am a sucker for a patch.
[^bm]: Coining the term **bumpermaxxing**
[^boop]: BOOP MY BELOVED
[^jj]: Not just a nod to Ubuntu's convention of alliterative release names, but also to the Jazz Jackrabbit video game which features a [difficulty selection screen that left a mark on me](https://bsky.app/profile/mif.fyi/post/3lxsoum6s7c2k).
[^mtp]: Including the time it took for me to bang my head against the Media Transfer Protocol.
[^rotate]: You may have noticed that I was slowly spinning around for some panels.
