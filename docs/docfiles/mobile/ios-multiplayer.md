---
sidebar_position: 5
---

# IOS Multiplayer
- To enable **Multiplayer** on a IOS device you must edit the **IOSEngine.ini** configuration file, this is the IOS equivilent to *DefaultEngine.ini*, copy-paste the settings from the *DefaultEngine.ini* to your **IOSEngine.ini** (*see the example below*).
- This example will allow you to connect using the EOS NAT P2P Interface and enable cross-platform multiplayer.

This example will allow you to connect using the EOS NAT P2P Interface and enable cross-platform multiplayer.

### Project\Config\IOS\IOSEngine.ini
:::danger NOTE
**Double check the location of your IOSEngine.ini configuration file, it must be located in Config\IOS\IOSEngine.ini**
:::
**IOSEngine.ini**
```c
[OnlineSubsystem]
DefaultPlatformService=EOSCore

[/Script/OnlineSubsystemEOSCore.NetDriverEOSCore]
NetConnectionClassName=OnlineSubsystemEOSCore.NetConnectionEOSCore
bIsUsingP2PSockets=true

[/Script/Engine.GameEngine]
!NetDriverDefinitions=ClearArray
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="/Script/OnlineSubsystemEOSCore.NetDriverEOSCore",DriverClassNameFallback="OnlineSubsystemUtils.IpNetDriver")
```