# Dedicated Server

:::tip TIP
This Example has a Video Tutorial that you can find here: [Related Video](../videos/dedicated-server.mdx)
:::

## Hosting the Server
You must host your dedicated game server on a remote machine/pc for everything to work correctly, firewalls and routers also has to be configured to allow UDP port 7777 (the default game port, or the port you pass with -Port=) for clients to connect successfully.

If you are hosting the server on the same PC you’re connecting from it will not work as expected because of how network routing works, please use a correct server setup as described above.

## Setting Up
Since Dedicated Servers don’t use **Relay Servers** we have to **disable** the **EOSCoreNetDriver**, simply removing the declarations will disable using the EOS NetDriver. 

## DefaultEngine.ini
Here is an example of what your DefaultEngine.ini should look like

```c
[Core.Log]
Online=Verbose
OnlineSession=Verbose
OnlineGame=Verbose
LogEOSCoreSubsystem=Verbose
LogEOSCore=Verbose

[OnlineSubsystem]
DefaultPlatformService=EOSCore

[/Script/Engine.GameEngine]
!NetDriverDefinitions=ClearArray
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="/Script/OnlineSubsystemUtils.IpNetDriver",DriverClassNameFallback="/Script/OnlineSubsystemUtils.IpNetDriver")

[/Script/OnlineSubsystemUtils.IpNetDriver]
MaxClientRate=1000000000 
MaxInternetClientRate=1000000000
InitialConnectTimeout=120.0

[EOSSDK]
bHasProjectBinary=true
```

### Launching the Server
We’ll use Commandline arguments to launch our server. `-EOSConfig=` is optional, use it if you have a separate configuration for your server in Project Settings (for example one that uses a server client policy). `-Port=` sets the game port, the plugin publishes it in the session so clients connect to the right port.

```
D:\WindowsServer\EOS_DedicatedServer.exe -log -Port=7777 -EOSConfig=ServerConfig
```

:::note
Automatic login with `-AUTH_TYPE=` only runs in Game and Client builds. In a build of a **Server** target it is ignored, so the server does not log in a user this way. If you run the server from a Game or Editor binary with `-server`, `-AUTH_TYPE=deviceid -AUTH_LOGIN=<display name>` logs the server in with a DeviceId.
:::

See [Commandline Arguments](../commandline_arguments.md) for all arguments.

## Listing the Server
You can use the default Session nodes to list a Dedicated Server. We’ll use the GameInstance in this example.

![Image](../../../static/img/83c7f976ec8accaf9796ac8fd21e338f.png)

This will List your dedicated server as a joinable session.

## Joining the Server
To join a dedicated server there are a few things you need to take into account:
- If you're hosting the server on your own **local network** you'll not be able to join the server via a server browser, this is because the server reports your **external IP address** to EOS and you can't join your own **local network** via your external ip address.
- If your server is hosted on your **local network** use the console to connect to it (see the sample below).
- Make sure that your client is [**logged in**](../getting_started/auth/information.md) before trying to connect to the server.

### Console Command
*Replace **127.0.0.1** with your servers IP address*
```cpp
open 127.0.0.1
```
![Image](../../../static/img/dedicated_server_console_command.png)