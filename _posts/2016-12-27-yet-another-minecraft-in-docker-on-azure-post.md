---
layout: post
title: Yet Another Minecraft in Docker on Azure Post (YAMIDOA)
---
It's time now for a little fun. After all, it's the Christmas and New Years season, so we need to find a way to learn something important whilst still having a good time!

To accomplish this goal, I am going to learn how to take a Minecraft server distro, get it running in a Docker container, then host it on Azure using an Ubuntu server (and given that I am normally quite Linux-challenged, this should be interesting). I could easily have chosen to stand up a Windows Server for this instead, but where's the fun in that?

## Getting Started

First things first: we're going to need some tools:
  *Docker for Windows (see my previous post for more info).
  *An Azure subscription (if you have MSDN you're covered, otherwise [check this out](https://azure.microsoft.com/en-us/free/))
  *[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to make an SSH connection to your Linux server
  *[WinSCP](https://winscp.net/eng/download.php) to transfer files to/from your Linux server
