# Getting the hang of Docker with .Net (Part 1)
I'll admit, I'm a little behind the curve when it comes to Docker and containers in general. In my current role, one of my responsibilities is making the work of managing deployments and releases as streamlined as possible, and yet it seems that I missed the ball with containers. So, I am jumping into it, and learning to both develop and deploy .Net applications using Docker containers.
## Preparations
First and foremost - you need Windows 10 (the Anniversary edition) or Windows 2016 Server. Earlier windows versions are not supported by Docker.

Next you need to get a copy of Docker. Luckily this is freely available at https://www.docker.com/.

Install Docker for Windows. Once installed, you can execute Docker commands from a variety of shell tools. I prefer PowerShell, but even cmd.exe will work. You can verify that you have Docker installed properly by entering this in your shell:
```
docker version
```
