---
title: SaltStack
date: 2016-01-08 19:40:14
---

[SaltStack](http://www.saltstack.com) is an opensource systems & configuration management software which is built on Python. During my time at Unity Technologies I've been the sole developer moving our whole build farm infrastructure from thick deployment disks (with everything pre-installed) to thin disks which install all required software automagically using SaltStack. 

Our SaltStack setup consists of many parts and some of our minions have over 100 states that are ran daily to keep the minions compatible with building our codebase. Our build farm includes minions running on Linux, Windows and OS X and each minion type has tens of different ways that they can be setup to make the compatible with the hundreds of different builds types that we run.

Most of the features developed for SaltStack for our buildfarm have made their way back to the project except for things that are not useful to the community at large.

## Features I've Worked On

- Android device beacon
- Permanent environment variables on Windows
- Power management on Windows
- Pythonized zip module
- Improvements to chocolatey modules

You can find Unity's fork of the project [here](https://github.com/Unity-Technologies/salt)