---
sidebar_position: 2
---

# Android Multiplayer
- To enable **Multiplayer** on a Android device you must edit the **AndroidEngine.ini** configuration file, this is the android equivilent to *DefaultEngine.ini*, copy-paste the settings from the *DefaultEngine.ini* to your **AndroidEngine.ini** (*see the example below*).
- This example will allow you to connect using the EOS NAT P2P Interface and enable cross-platform multiplayer.


### Project\Config\Android\AndroidEngine.ini
:::danger NOTE
**Double check the location of your AndroidEngine.ini configuration file, it must be located in Config\Android\AndroidEngine.ini**
:::
**AndroidEngine.ini** 
```c
[OnlineSubsystem]
DefaultPlatformService=EOSCore

[/Script/OnlineSubsystemEOSCore.NetDriverEOSCore]
NetConnectionClassName=OnlineSubsystemEOSCore.NetConnectionEOSCore
bIsUsingP2PSockets=true

[/Script/Engine.GameEngine]
!NetDriverDefinitions=ClearArray
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="OnlineSubsystemEOSCore.NetDriverEOSCore",DriverClassNameFallback="OnlineSubsystemUtils.IpNetDriver")
```