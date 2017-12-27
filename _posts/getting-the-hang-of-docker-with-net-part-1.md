# Getting the hang of Docker with .Net (Part 1)
I'll admit, I'm a little behind the curve when it comes to Docker and containers in general. In my current role, one of my responsibilities is making the work of managing deployments and releases as streamlined as possible, and yet it seems that I missed the ball with containers. So, I am jumping into it, and learning to both develop and deploy .Net applications using Docker containers.
## Preparations
First and foremost - you need Windows 10 (the Anniversary edition) or Windows 2016 Server. Earlier windows versions are not supported by Docker.

Next you need to get a copy of Docker. Luckily this is freely available at https://www.docker.com/.

Install Docker for Windows. Once installed, you can execute Docker commands from a variety of shell tools. I prefer PowerShell, but even cmd.exe will work. You can verify that you have Docker installed properly by entering this in your shell:
```
docker version
```
If everything is working, you should see something similar to:
```
Client:
 Version:      1.13.0-rc3
 API version:  1.25
 Go version:   go1.7.3
 Git commit:   4d92237
 Built:        Tue Dec  6 01:15:44 2016
 OS/Arch:      windows/amd64

Server:
 Version:      1.13.0-rc3
 API version:  1.25 (minimum version 1.24)
 Go version:   go1.7.3
 Git commit:   4d92237
 Built:        Tue Dec  6 01:15:44 2016
 OS/Arch:      windows/amd64
 Experimental: false
```
## Get a base image
Since we're working with Windows, we'll need to pull down a Windows container image. Type the following into your shell:
```
docker pull microsoft/windowsservercore
```
This will pull the latest Windows Server base image from the web. You should see something siliar to:
```
Using default tag: latest
latest: Pulling from microsoft/windowsservercore
9c7f9c7d9bc2: Pull complete
d33fff6043a1: Pull complete
Digest: sha256:571381e3e11fb93ba128adbdcaeb354cb6a470ee60919b89627731361b021d6e
Status: Downloaded newer image for microsoft/windowsservercore:latest
```
Run a quick test of the image by typing this into your shell:
```
docker run microsoft/windowsservercore hostname
```
It will churn for a while, then return:
```
6754a75286e0
```
## Build and push the Windows container image
In order to do this, you'll need to have a Docker ID - this is free and can be obtained from https://cloud.docker.com.

Once you have your Docker ID, enter this into your shell:
```
"FROM microsoft/windowsservercore `n CMD echo Hello World!" | docker build -t <docker-id>/windows-test-image -
```
This will create a running instance of your container on Docker. You can test this image by entering this into your shell:
```
docker run <docker-id>/windows-test-image
```
Which will reward you for your efforts by returning:
```
Hello World!
```
Great! Your container is running and responds as expected. Now you can push this image to the Docker Cloud, which will make it available for other Docker users.

Make sure you have logged in to Docker Cloud first, then enter:
```
docker push <docker-id>/windows-test-image
```
You should see something like this:
```
The push refers to a repository [docker.io/<docker-id>/windows-test-image]
ffbf052bf652: Pushed
b9454c3094c6: Skipped foreign layer
3fd27ecef6a3: Skipped foreign layer
latest: digest: sha256:074c88a06222ac75880749b4c65db59f103f01eb94dbdee35b5cb182e51a7b95     size: 1152
```
Now look out on Docker Cloud, in the Repositories tab, and you should see your new image:

![Docker Cloud Image](https://raw.githubusercontent.com/tdallmann/tdallmann.github.io/master/images/DockerRepo.PNG "Docker Cloud Image")
## Conclusion
In this post we took some small steps to utilize Docker. As we move through the series we will continue to expand on this towards the goal of using Docker to run a .Net application and deploy to a cloud server like Azure.
