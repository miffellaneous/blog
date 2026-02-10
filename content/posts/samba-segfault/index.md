+++
title = "Samba: volume_label segfault with bad usershare permissions"
date = "2026-02-10T19:00:00Z"
author = "Miffy"
authorTwitter = "" #do not include @
cover = "cover.png"
coverCaption = "Sweat baby, sweat baby, NAS is in segment fault"
coverCreditUrl = "https://albys.space/about/"
coverCreditName = "Alby"
tags = ["tech", "samba", "debug"]
#keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
summary = "In this post I discover I set up Samba wrong, triggering an unexpected segfault."
+++

## Background

A few weeks ago I moved the last of my local [Obsidian Vaults](https://obsidian.md/) to my NAS.
Almost immediately after moving the vault, while editing a note on my desktop, Obsidian unceremoniously closed the vault and quit to the vault management screen.

Odd, I thought, until it happened again minutes later; and many times, unrelentingly in the following hours and days after.

This was disappointing, not just because a computer was misbehaving under mysterious circumstances; but I had just migrated both the vault I share with my partner to the NAS, and their personal vault, after laboriously extolling the virtues of the NAS I had built[^nas].
The NAS was supposed to make sharing more reliable and straightforward, to be the perfect system, not to bring my systems into question and just generally harsh the vibes, man.

It was curious that I had only encountered this issue now.
I built the NAS several months ago and have been using it for all sorts of things and had not seen instability before.
Although it was Obsidian that triggered this behaviour for the first time, I immediately turned my suspicions to the layer between my Windows speaking desktop and the NAS: [Samba](https://www.samba.org/).


## Error

Neatly, Samba retains individual logs for each connected client.
I `tail`ed the log for my desktop and fidgeted around Obsidian waiting for another crash, and sure enough: observed a segfault in `smbd`!

```
[2026/01/20 18:45:30.577215,  0] lib/util/fault.c:192(smb_panic_log)
  PANIC (pid 1168495): Signal 11: Segmentation fault in 4.19.5-Ubuntu
[2026/01/20 18:45:30.577462,  0] lib/util/fault.c:303(log_stack_trace)
  BACKTRACE: 29 stack frames:
   #0 /usr/lib/x86_64-linux-gnu/samba/libgenrand-samba4.so.0(log_stack_trace+0x37) [0x7084668a4517]
   #1 /usr/lib/x86_64-linux-gnu/samba/libgenrand-samba4.so.0(smb_panic+0x15) [0x7084668a4d25]
   #2 /usr/lib/x86_64-linux-gnu/samba/libgenrand-samba4.so.0(+0x2dca) [0x7084668a4dca]
   #3 /lib/x86_64-linux-gnu/libc.so.6(+0x45330) [0x708466645330]
   #4 /lib/x86_64-linux-gnu/libc.so.6(+0x18b79d) [0x70846678b79d]
   #5 /lib/x86_64-linux-gnu/libsmbconf.so.0(volume_label+0x53) [0x708466bad963]
   #6 /usr/lib/x86_64-linux-gnu/samba/libsmbd-base-samba4.so.0(smbd_do_qfsinfo+0xa2) [0x708466c7fc82]
   #7 /usr/lib/x86_64-linux-gnu/samba/libsmbd-base-samba4.so.0(smbd_smb2_request_process_getinfo+0x276) [0x708466ce2096]
   #8 /usr/lib/x86_64-linux-gnu/samba/libsmbd-base-samba4.so.0(smbd_smb2_request_dispatch+0x10c7) [0x708466cc7d07]
   #9 /usr/lib/x86_64-linux-gnu/samba/libsmbd-base-samba4.so.0(smbd_smb2_request_dispatch_immediate+0x59) [0x708466cc9439]
   #10 /lib/x86_64-linux-gnu/libtevent.so.0(tevent_common_invoke_immediate_handler+0x170) [0x708466837080]
   #11 /lib/x86_64-linux-gnu/libtevent.so.0(tevent_common_loop_immediate+0x22) [0x7084668370e2]
   #12 /lib/x86_64-linux-gnu/libtevent.so.0(+0xed92) [0x70846683ad92]
   #13 /lib/x86_64-linux-gnu/libtevent.so.0(+0x6004) [0x708466832004]
   #14 /lib/x86_64-linux-gnu/libtevent.so.0(_tevent_loop_once+0x9b) [0x708466833bab]
   #15 /lib/x86_64-linux-gnu/libtevent.so.0(tevent_common_loop_wait+0x2b) [0x708466833cfb]
   #16 /lib/x86_64-linux-gnu/libtevent.so.0(+0x6084) [0x708466832084]
   #17 /usr/lib/x86_64-linux-gnu/samba/libsmbd-base-samba4.so.0(smbd_process+0x7eb) [0x708466cb534b]
   #18 smbd: client [10.10.8.6](+0xa5d6) [0x5f68623115d6]
   #19 /lib/x86_64-linux-gnu/libtevent.so.0(tevent_common_invoke_fd_handler+0x98) [0x708466836e48]
   #20 /lib/x86_64-linux-gnu/libtevent.so.0(+0xefda) [0x70846683afda]
   #21 /lib/x86_64-linux-gnu/libtevent.so.0(+0x6004) [0x708466832004]
   #22 /lib/x86_64-linux-gnu/libtevent.so.0(_tevent_loop_once+0x9b) [0x708466833bab]
   #23 /lib/x86_64-linux-gnu/libtevent.so.0(tevent_common_loop_wait+0x2b) [0x708466833cfb]
   #24 /lib/x86_64-linux-gnu/libtevent.so.0(+0x6084) [0x708466832084]
   #25 smbd: client [10.10.8.6](main+0x1432) [0x5f686230f032]
   #26 /lib/x86_64-linux-gnu/libc.so.6(+0x2a1ca) [0x70846662a1ca]
   #27 /lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0x8b) [0x70846662a28b]
   #28 smbd: client [10.10.8.6](_start+0x25) [0x5f686230fa95]
[2026/01/20 18:45:30.577562,  0] source3/lib/util.c:691(smb_panic_s3)
  smb_panic(): calling panic action [/usr/share/samba/panic-action 1168495]
[2026/01/20 18:45:30.579285,  0] source3/lib/util.c:698(smb_panic_s3)
  smb_panic(): action returned status 0
```

It would appear that in this [`volume_label` function](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c?ref_type=tags#L4385), we're [calling `strlen` on something that is `NULL`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c?ref_type=tags#L4403); widely regarded as a crime worthy of a segmentation fault.
But here is where things get interesting, `volume_label` deferences pointers in such a way that it clearly expects that neither `lp_volume` or `lp_servicename` can return `NULL`, so what happened?

## Security contexts

Following the [Samba team's guidance on bug reporting](https://www.samba.org/~asn/reporting_samba_bugs.txt), I dialled up logging to 10 in `smb.conf` and an hour later I'd captured several gigabytes of indecipherable text, dotted with the occasional segmentation fault.
Here's one, ironically encountered while trying to edit my Obsidian note about these very errors.

```
[2026/01/22 23:50:56.079345, 10, pid=1225061, effective(8000, 8000), real(8000, 0), class=smb2] source3/smbd/smb2_getinfo.c:277(smbd_smb2_getinfo_send)
                                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  smbd_smb2_getinfo_send: obsidian/miffy/investigations/2026-01_smbd_segfautls.md - fnum 1247396707
[2026/01/22 23:50:56.079392,  0, pid=1225061, effective(8000, 8000), real(8000, 0)] lib/util/fault.c:192(smb_panic_log)
  PANIC (pid 1225061): Signal 11: Segmentation fault in 4.19.5-Ubuntu
```

My attention was drawn to the real and effective permissions that are not presented in the regular log level.
I can see that `smbd` switches through root and user security contexts in the log via calls to `change_to_root_user` and `change_to_user_impersonate` in my log.
Now, I can imagine that the intermittent nature of the segfault *could* be explained by the changing security contexts.

## Permission errors
Here's where things get interesting.
While inspecting the log for clues, I discovered I'd also been encountering non-fatal permission denied errors:

```
[2026/01/22 23:50:48.084740,  0, pid=1225061, effective(8000, 8000), real(8000, 0)] source3/param/loadparm.c:3480(process_usershare_file)
  process_usershare_file: stat of /var/lib/samba/usershares/a_b failed. Permission denied
```

Not only that, but crucially, these permission errors only occur when `smbd` has used `change_to_user_impersonate` to execute in the non-root security context:

```
$ grep -B1 --no-group-separator 'stat of /var/lib/samba/usershares/a_b' _client.log | grep -v 'Permission denied' | awk '{ print $5,$6,$7,$8 }' | sort | uniq -c
    147 effective(65534, 65534), real(65534, 0)]
    113 effective(8000, 8000), real(8000, 0)]
```

Facepalm.
Indeed, my Samba user really had insufficient permissions to read any file in the `/var/lib/samba/usershares/` directory, because I had not added them to the `sambashare` group.
I had created my Samba users with `adduser` and provided access to SMB with `pdbedit`, but not realised that ZFS' `sharesmb` option would require users to be in the `sambashare` group too.

I added our Samba users to the `sambashare` group two weeks ago and have not had a segfault since.
As an experiment, I reverted this change and immediately triggered a segfault in `smbd` that caused data loss, necessitating a successful unscheduled test of our ZFS snapshot system[^httm].


## Segfault: Redux
So I made the segfault go away, Obsidian has stopped crashing and my reputation as a system administrator is largely unblemished, of course, this was not good enough for me.
This curious bun **had to know** why a seemingly innocuous permission error leads to a segmentation fault.

So here's the fun part, thanks to the detailed logging, I was able to take a good look at how this crash may occur.
I had no familiarity with the Samba codebase until investigating this issue, so the obvious caveats aside, here is the chain of events that I believe leads to the segfault:

- Business as usual, and we are drop down to a non-root security context

- During some request, we process the path with `get_referred_path` to [split out the path components with `parse_dfs_path_strict`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/smbd/msdfs.c#L938) and [determine the service number related to the SMB path with `lp_servicenumber`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/smbd/msdfs.c#L964)
- [`lp_servicenumber` checks the usershare still exists with `usershare_exists`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L4354)
- But [`usershare_exists` silently fails if `sys_lstat` fails when reading the usershare file](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L3646), which it would if we do not have permission to stat it
- `usershare_exists` exits false, causing the service to be [freed by `lp_servicenumber` using `free_service_byindex`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L4358)
- As logged, the service corresponding to the usershare we are trying to access is [freed by `free_service` called via `free_service_byindex`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L1430)
- As a result the `ServicePtrs[idx]` entry corresponding to the usershare has been freed and is now `NULL`
- We're *still* in `get_referred_path`, but failed to lookup the service with `lp_servicenumber`, so [call `find_service` to... find the service we just freed](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/smbd/msdfs.c#L967)
- [`find_service` tries to determine if this is a usershare with `load_usershare_service`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/service.c#L206) which in turn [attempts to parse the usershare file into a service with `process_usershare_file`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L3744)
- But the [usershare file cannot be stat](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L3480) and `find_service` [fails to find our usershare service](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/service.c#L261), and we exit `get_referred_path` with nothing but a `NULL` where our service used to be
- Some time passes... A new request from a client comes in and [`smbd_do_qfsinfo` calls `volume_label`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/smbd/smb2_trans2.c#L2051), presumably using a stale service number as the client has no idea the service has been freed[^guess]
- As part of `qfsinfo`, the [`volume_label` function interrogates `lp_volume`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L4390) and [`lp_servicename`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L4394) to have a nice human label for the client's request
- My now quite stretched understanding is that `lp_volume` is [a `FN_LOCAL_SUBSTITUTED_STRING` macro](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L1101), and its definition is [automatically generated at compile-time into `param_table_gen.c`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/lib/param/wscript_build#L27) from the [corresponding docs](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/docs-xml/smbdotconf/misc/volume.xml)
- The `FN_LOCAL_SUBSTITUTED_STRING` macro checks for a valid service and either returns the service's field, or falls back to the default value; before handling any substituting for its final value. As our service was freed, the `LP_SNUM_OK(i)` check for our service will be `false`, and for `lp_volume` we'll fallback to `sDefault.volume`
- At first glance [the default for the `volume` field in the default service definition is `NULL`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L161), but `init_globals` loops over the previously mentioned autogenerated compile-time definitions and [sets any NULL string defaults in the default service definition to `lpcfg_string_empty` ("") with `lpcfg_string_set`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L544). It took me quite a while to figure out [`lp_parm_ptr` is how `init_globals`' call to `lpcfg_string_set` writes into the default service definition](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L2683).
- So `lp_volume` is an empty string for our freed service after all, and [`volume_label` moves on to try `lp_servicename`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L4394) instead
- `lp_servicename` is [similarly an `FN_LOCAL_SUBSTITUTED_STRING` macro](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L1135) but does not have any autogenerated compile-time entries in `parm_table` and is not initialised by `init_globals` like `volume` either. I believe it therefore retains its [compile time default of `NULL`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L127)
- Falling back to the `NULL` default, `lpcfg_substituted_string` uses `loadparm_s3_global_substitution_fn` to determine the final output value for `lp_servicename`; which [still returns `NULL` if its input is `NULL`](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L1054)
- And so, `volume_label` [fatally calls `strlen` on the `NULL` `lp_servicename` result, causing the segmentation fault](https://gitlab.com/samba-team/samba/-/blob/samba-4.19.5/source3/param/loadparm.c#L4403)

## tl;dr

- Obsidian began crashing on Windows clients after migrating vaults to my NAS
- My partner questioned why they were with someone who could not operate Samba
- It turned out that Samba was segfaulting, seemingly due to permission errors
- I made it stop segfaulting by granting permissions previously not granted
- I couldn't leave things alone and spent several evenings constructing a plausible chain of events to share with the Samba mailing list and my internet animal friends

I've [presented my evidence to the Samba developers](https://bugzilla.samba.org/show_bug.cgi?id=14978#c13) (a little more dryly than I have here), and will update this post if I hear anything back.
If you've encountered this issue and it was user permissions all along (or not), please let me know and update the Samba ticket!


[^nas]: I'll write about it some time, I keep telling myself.
[^httm]: I'll write this up one day too, but a deserved [shoutout to `httm`](https://github.com/kimono-koans/httm) for making the exploration and restoration of files from ZFS snapshots very easy.
[^guess]: This is the part I am a little fuzzy on, but I'm compelled by the rest of the chain of events to believe this is plausible.
