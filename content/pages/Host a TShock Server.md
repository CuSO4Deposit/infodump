---
tags:
- Terraria
- TShock
date: 2023-07-16
title: Host a TShock Server
categories:
lastMod: 2025-10-18
--- 
## Install

Hosting a tshock server is similar to a original terraria server. The only difference is that you need to install dotnet.

```shell
> arch
x86_64
> curl -L "https://github.com/Pryaxis/TShock/releases/download/v5.2.0/TShock-5.2-for-Terraria-1.4.4.9-linux-x64-Release.zip" -o ./TShock-5-2.zip
> unzip ./TShock-5-2.zip
> tar -xvf ./Tshork-Beta-linux-x64-Release-tar
> ./TShock.Server
```

Then the server will prompt you to install dotnet.

[Install .NET on Linux distributions - .NET | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/core/install/linux)

```shell
> wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
> sudo dpkg -i packages-microsoft-prod.deb
> rm packages-microsoft-prod.deb
> sudo apt-get update && sudo apt-get install -y aspnetcore-runtime-6.0
> ./TShock.Server
```

[Troubleshooting]

After I did the above on Ubuntu 22.04, TShock still cannot find .NET, but `apt` says dotnet is already installed. Here an environment variable should be set. And be sure where to point the env, in my case it is `/usr/lib/dotnet/`.



[.net - dotnet sdk is installed but not recognized - Linux Ubuntu/popOS 22.04 - Stack Overflow](https://stackoverflow.com/questions/73312785/dotnet-sdk-is-installed-but-not-recognized-linux-ubuntu-popos-22-04/76432110#76432110)
  
## Configure

The configuration process needs to start a terraria game and connect to this server. The server console will show a setup code like `/setup 114514`. Use a random player and connect to the server, and type `/setup [code]` in the game chat. Then use `/user add [username] [password] owner` to add an admin, and `/login [username] [password]`. Then `/setup` finishes this configuration.
 