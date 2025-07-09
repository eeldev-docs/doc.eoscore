---
sidebar_position: 3
---

# Configuring Online Subsystem

## Overview

This guide provides instructions for configuring the EOS Online Subsystem in your project. Proper configuration is essential for establishing network connectivity and multiplayer functionality.

## Prerequisites

:::warning Important
Before proceeding, verify that your `DefaultEngine.ini` file does not contain any duplicate entries for the configurations listed below. Duplicate entries will cause configuration conflicts and prevent proper functionality.
:::

## Video Tutorial

For a visual walkthrough of this process, please refer to our [Video Tutorial](../videos/installing-and-configuring.mdx).

## Configuration Steps

### 1. Locate Configuration File

Navigate to your project's configuration directory and open the `DefaultEngine.ini` file located at:
```
Project\Config\DefaultEngine.ini
```

### 2. Basic Configuration

Add the following configuration to your `DefaultEngine.ini` file. Ensure that you update the `DefaultPlatformService` to use `EOSCore` instead of any previously configured service (such as Steam).

```ini
[OnlineSubsystem]
DefaultPlatformService=EOSCore

[/Script/OnlineSubsystemEOSCore.NetDriverEOSCore]
NetConnectionClassName="/Script/OnlineSubsystemEOSCore.NetConnectionEOSCore"
bIsUsingP2PSockets=true

[/Script/Engine.GameEngine]
!NetDriverDefinitions=ClearArray
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="/Script/OnlineSubsystemEOSCore.NetDriverEOSCore",DriverClassNameFallback="OnlineSubsystemUtils.IpNetDriver")
```

This configuration establishes the foundation for SteamSockets functionality within the EOS framework.

## Performance Optimization (Optional)

### Network Rate Limiting

To optimize network performance, you may configure additional settings for the `NetDriverEOSCore`. These settings help manage data transmission rates between clients and servers, preventing network congestion that can occur with high frame rate applications.

#### Benefits
- Prevents clients from overwhelming servers with excessive data packets
- Establishes consistent communication rates regardless of application frame rate
- Reduces packet loss due to network saturation

#### Configuration
```ini
[/Script/OnlineSubsystemEOSCore.NetDriverEOSCore]
NetConnectionClassName="/Script/OnlineSubsystemEOSCore.NetConnectionEOSCore"
MaxNetTickRate=60
NetServerMaxTickRate=60
LanServerMaxTickRate=60
NetClientTicksPerSecond=60
bClampListenServerTickRates=true
MaxClientRate=100000
MaxInternetClientRate=100000
```

### Parameter Explanation

- **MaxNetTickRate**: Maximum network update frequency (60 Hz recommended)
- **NetServerMaxTickRate**: Server-side maximum tick rate
- **LanServerMaxTickRate**: LAN-specific maximum tick rate
- **NetClientTicksPerSecond**: Client-side network update frequency
- **bClampListenServerTickRates**: Enables rate limiting for listen servers
- **MaxClientRate**: Maximum data rate for client connections (bytes per second)
- **MaxInternetClientRate**: Maximum data rate for internet-based connections

## Additional Resources

For detailed explanations of individual network settings and advanced configuration options, please consult the official engine documentation.