---
sidebar_position: 3
---

# Configuring Online Subsystem

:::tip NOTE
Make sure you don’t already have any of the below defintions already defined in your DefaultEngine.ini, duplicate entries will break the configuration
:::

Edit your DefaultEngine.ini file located in **Project\Config\DefaultEngine.ini** and add the following configuration, remember to change out the DefaultPlatformService to EOS (if previously using Steam or any other Service)

## DefaultEngine.ini
Edit your **DefaultEngine.ini** configuration file located in the Project\Config directory. The example below is all you need for a basic SteamSockets setup.
```c
[OnlineSubsystem]
DefaultPlatformService=EOSCore

[/Script/OnlineSubsystemEOSCore.NetDriverEOSCore]
NetConnectionClassName="OnlineSubsystemEOSCore.NetConnectionEOSCore"
bIsUsingP2PSockets=true

[/Script/Engine.GameEngine]
!NetDriverDefinitions=ClearArray
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="OnlineSubsystemEOSCore.NetDriverEOSCore",DriverClassNameFallback="OnlineSubsystemUtils.IpNetDriver")
```

## Optimizing (Optional)
There are a few settings that you can set on the **NetDriverEOSCore** to fine-tune your connection settings, see the engine documentation for explanation on the different settings.
- *These settings will limit at what rates client and server talk to each other, because of very high framerates (FPS) sometimes the client will hammer the server with data, too much data that it will eventually get dropped.*
- *The settings above will limit the send rate to “60 FPS” rather than spamming the server at the games FPS (100-200++ frames per second)*

```c
[/Script/OnlineSubsystemEOSCore.NetDriverEOSCore]
NetConnectionClassName="OnlineSubsystemEOSCore.NetConnectionEOSCore"
MaxNetTickRate=60
NetServerMaxTickRate=60
LanServerMaxTickRate=60
NetClientTicksPerSecond=60
bClampListenServerTickRates=true
MaxClientRate=100000
MaxInternetClientRate=100000
```