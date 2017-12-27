---
layout: post
title: Yet Another Minecraft in Docker on Azure Post (YAMIDOA)
---
It's time now for a little fun. After all, it's the Christmas and New Years season, so we need to find a way to learn something important whilst still having a good time!

To accomplish this goal, I am going to learn how to take a Minecraft server distro, get it running in a Docker container, then host it on Azure using an Ubuntu server (and given that I am normally quite Linux-challenged, this should be interesting). I could easily have chosen to stand up a Windows Server for this instead, but where's the fun in that?

## Getting Started

First things first: we're going to need some tools:
  * Docker for Windows (see my previous post for more info).
  * An Azure subscription (if you have MSDN you're covered, otherwise [check this out](https://azure.microsoft.com/en-us/free/))
  * [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to make an SSH connection to your Linux server
  * [WinSCP](https://winscp.net/eng/download.php) to transfer files to/from your Linux server

### A Minecraft Server Image
Next, head over to https://hub.docker.com/r/itzg/minecraft-server/ where you will find a very nice server distribution for Minecraft, all set up to run in Docker. The nice part about this distro is that it's very configurable, and it will allow you to keep your server version up to date with very little effort.

There is a lot of good stuff on this page that I won't repeat here - I encourage you to try things out on your command line and experiment.

### First Step - Run Locally
Open your shell tool of choice (I like PowerShell) and enter the command:
```
docker pull itzg/minecraft-server
docker run -d -p 25565:25565 --name mc itzg/minecraft-server
```
This grabs the image from the Docker site, then runs it with the name "mc". Want to know for sure it's running? Just use:
```
Docker ps -a
```
Which results in somethign like this: 
