# Danny's Documentation: AWS EC2 Instances and Game Servers
Hi, and welcome to Danny's Documentation! A series where I document my personal projects. This file is for learning how to properly set up and run your very own AWS EC2 Instance and/or set up a game server on it!

## Table of Contents
[Setting Up Minecraft Server On Linux](#setting-up-minecraft-server-on-linux)  
[Setting Up Terraria Server On Linux](#setting-up-terraria-server-on-linux)

## Setting Up Minecraft Server On Linux
Let's start with some EC2 Knowledge. By default, EC2 Instances have Java 7 which doesn't work with more modern minecraft versions. We need to remove java 7, then install and configure java 8.

Remove java 7 first, to avoid conflicts  
`sudo yum remove java-1.7`

Install the open source version of java 8  
`sudo yum install -y java-1.8.0-openjdk.x86_64`

Set new `java` and `javac` paths  
`sudo /usr/sbin/alternatives --set java /usr/lib/jvm/jre-1.8.0-openjdk.x86_64/bin/java`  
`sudo /usr/sbin/alternatives --set javac /usr/lib/jvm/jre-1.8.0-openjdk.x86_64/bin/javac`

Create folder for minecraft  
`mkdir minecraft_server`  
`cd minecraft_server`

Download minecraft server client. At the time of writing this, current version is 1.13  
`sudo wget https://launcher.mojang.com/mc/game/1.13/server/d0caafb8438ebd206f99930cfaecfa6c9a13dca0/server.jar`  
Note: This download link might not work. If it does not, you will need to either find your own link (which was hard for me), or download the server.jar through [mojang](https://minecraft.net/en-us/download/server) on your own personal computer and use pscp, from [PuTTY](https://www.putty.org/), to copy the file to the server. Briefly, pscp is PuTTY's version of the linux Secure Copy, [scp](https://en.wikipedia.org/wiki/Secure_copy).

Run minecraft server without user interface  
`java -jar server.jar nogui`

For a first time run, it will create files, and then fail to start. Use your preferred editor to open `eula.txt` and change `false` to `true`.

Rerun, and happy building!

## Setting Up Terraria Server On Linux
First step, we want to download a library that allows us to use mono.
Mono is an open implementation of Microsoft's .NET framework. This means that if an application is built on that framework, it can be run through mono. This allows us to run specific `.exe` files in linux environments.

This was the only website I could find that had the `libpng15-1.5.28-2.fc26.x86_64.rpm` file ([link](archives.fedoraproject.org/pub/archive/fedora/linux/releases/26/Everything/x86_64/os/Packages/l/libpng15-1.5.28-2.fc26.x86_64.rpm)). If the link doesn't work anymore google for a new one.  
`wget archives.fedoraproject.org/pub/archive/fedora/linux/releases/26/Everything/x86_64/os/Packages/l/libpng15-1.5.28-2.fc26.x86_64.rpm`

Install the library from the download  
`sudo yum install -y libpng15-1.5.28-2.fc26.x86_64.rpm`

Remove the download  
`sudo rm libpng15-1.5.28-2.fc26.x86_64.rpm`

Now that we have the library,  we can install mono  
`sudo yum install mono`

After mono is correctly installed, we need to download the tshock release of terraria. [TShock](https://github.com/Pryaxis/TShock) is server software offering great customization and versatility for terraria servers.
https://github.com/Pryaxis/TShock/releases

At the current time of writing, the most recent release is tshock_4.3.25.  
`wget https://github.com/Pryaxis/TShock/releases/download/v4.3.25/tshock_4.3.25.zip`  

Unzip and create folder called tshock  
`unzip tshock_4.3.25.zip -d tshock/`

Remove tshock zip  
`sudo rm tshock_4.3.25.zip`

Now run your server!  
`cd tshock`  
`mono TerrariaServer.exe`

It will prompt you to choose settings for the world size, max players, etc. Once you choose your options it will start up!
