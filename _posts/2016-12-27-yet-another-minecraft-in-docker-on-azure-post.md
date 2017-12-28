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
Which results in something like this: 
![powershell output](/images/ps.PNG)

At this point you can also validate that there really is a Minecraft server running on your local machine by starting Minecraft, click Multiplayer, then Direct Connect, and enter "localhost:25565" as the server address. Click Join Server, and you should be rewarded by entering your new world.

Not working? The easiest way to see what's happening is to attach to the container:
```
docker attach mc
```
Try connecting again and you will see any error messages that are going to standard output.

## Your Azure VM
Now that we've proven the Minecraft server can run locally in a Docker container, we can perform the same basic steps on an Azure VM. So let's set one up:

  * Log in to the Azure Portal
  * (optional) Create a new Resource Group
    * Resource groups are a helpful way to organize Azure objects
  * Add a new VM to the resource group. Search for "Docker on Ubuntu Server", which is a VM image already set up for Docker.
    * Give it a name
    * Provide a user name and Password
    * Select a pricing tier
    * Under "Optional Configuration" --> "Endpoints", add an endpoint for Minecraft on port 25565

### Connect to Your Azure VM
Now it's time to use PuTTY to connect to the VM you just created. Start PuTTY and enter the IP address of your new VM. It will prompt you for your user name and password (which you entered when you created the VM).

## The Minecraft Server
You should now see a command prompt connected to the VM. Enter the following command:
```
docker run -d -it -e EULA=TRUE -p 25565:25565 --name mc itzg/minecraft-server
```
This is doing a couple things:
  * **-d** Run "detached". This means that after the command is complete, you'll be returned to the command prompt, rather than tying up the session.
  * **-it** Allow for an interactive session. You can attach to the Minecraft container and view standard out as well as enter Minecraft server commands.
  * **-e EULA=TRUE** suppress the prompt to agree to the end user license agreement (Minecraft requires that this be agreed to before a server will run).
  * **-p 25565:25565** Map the containers port 25565 to port 25565 on the server
  * **--name mc** Name the container "mc"
  
Just like on your local machine, the container image should download and extract to the VM.

Test it out by using the Minecraft client.

### Server Commands
As I mentioned before, you can use PuTTY for an interactive session, since we used the "-it" option. To so this, enter the following command:
```
docker attach mc
```
You may not see anything happening immediately, but try entering a Minecraft server command. For example:
```
/op Timmolator
```
This will add the user Timmolator to the operators list.

If you go back to Minecraft and connect to the server, you should see yourself logging into the server. Likewise, when you leave the game, it will display in the log.

Leave interactive mode by using Ctrl-p then Ctrl-q.

Technically that's it - you're running a Minecraft server hosted in a Docker container on Linux. You can stop here if you want, but ...

## Mods
Let's face it - running a Minecraft server without mods is pretty boring. I like to use Forge to provide mod-running capability to a Minecraft server. I won't go into all the details about using Forge here, but I will show you how to set up your server to use Farge, and how to manage the mods you want to add to the server.

### Including Forge
We need to add another option to our command line:
```
-e TYPE=FORGE 
```
This tells the Minecraft container image that you want to use the recommended version of Forge.

### Map the Data Drive
At the same time, we're going to add one more option:
```
-v /path/on/host:/data
```
This maps a location on the VM's hard drive to the /data folder in the container, effectively storing the contents on the VM, rather than in the container itself. This is how you will be able to add and update mod files.

Our new startup command looks like this:
```
docker run -d -e EULA=TRUE -p 25565:25565 --name mc -e TYPE=FORGE -v /home/username/mc/data:/data itzg/minecraft-server
````
If you try to run this before removing the existing container, you'll get an error from Docker. Either use a different name to create a second container, or issue the following commands to remove the old container:
```
docker stop mc
docker rm mc
```
Also, if your container is terminating right after starting (so you can't attach to it), simply remove the **-d** option - you will see any errors that are preventing the server from starting.

### Managing Mods
To view the Data folder, where all the mode files live, you'll need to use WinSCP. You use the same credentials to connect to the server that you used for PuTTY.

Once connected, you can navigate to /home/username/mc/data and see all the files.

After downloading a mod to your local machine, use WinSCP to upload it to the mods folder. Then simply run this in PuTTY:
```
docker stop mc
docker start mc
```
Of course, you'll need to make sure Forge and the mods are installed on your client as well. There's nothing different about this just because we're using Docker on Azure.

## Codifying the Configuration
While this is all well and good, you may be wondering if there is a better way to manage this than simply specifying all the options on the command line. After all, DevOps is all about turning infrastructure into code, right? Well, there is a way, and I'll now show you how!

### Docker-Compose
The trick is to create a docker-compose file and put it on the server. Mine looks like this (located in the /home/username folder):

```
minecraft-server:
  ports:
    - "25565:25565"

  environment:
    EULA: "TRUE"
    TYPE: "FORGE"
    DIFFICULTY: "normal"
    WHITELIST: "Timmolator"
    OPS: "Timmolator"
    ENABLE_COMMAND_BLOCK: "TRUE"
    VIEW_DISTANCE: "10"

  volumes:
    - /home/tdallmann/mc/data:/data

  image: itzg/minecraft-server

  container_name: mc

  tty: true
  stdin_open: true
  restart: always
```
See the [Docker-Compose Overview](https://docs.docker.com/compose/overview/) for more information about this utility.

You can see many of the same options we used earlier in the command line, now present in this file. When you use a docker-compose file, it will build/rebuild a container and its services. When you run it for the first time, use:
```
docker-compose up -d
```
This will build and start the services contained in the file (minecraft-server in this instance).

## Wrapping Up
I had some fun putting this together - I hope you did too! It was nice to take a break from "real" issues and focus on something a little lighter.
