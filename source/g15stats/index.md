---
title: G15 Stats
date: 2016-01-16 12:29:28
---

While creating & playing games I’m always curious to see some certain stats about my PC, things like CPU load and RAM usage so I can tell whether something is going wrong or whether a games usage is too high. This was possible using some applications for the LCD on my G15 Keyboard that came with the keyboard, however they didn’t include all the information that I wanted to know, for example there was no hard drive usage value, or network usage.

So I went about building my own application for the LCD on my G15 keyboard. I used the EZ_LCD wrapper from the SDK that came with the keyboard and a few extra classes that I made to grab the information for the CPU, Network Card, Ram & Hard Drives. The application also includes some basic settings so that you can select which Network card or hard drives to view on the screen.

Unfortunately to actually see the workings of this application you **WILL** need a G15 keyboard yourself, but I have made a video and taken some photos of the application running on my PC below.

**Update**: After some requests I have updated G15 Stats with the ability to show what song your currently playing on Spotify.
**Update 2**: I have worked on an update to the G15 which includes a colour display for the G19 keyboard

A video running through the features of the keyboard
{% youtube v48AuWqmnHY %}

A simulation of the G15 keyboard running G15Stats
{% youtube FRCd3pRZJRM %}

## Project Details

- Language: C++
- Libraries: EZ_LCD