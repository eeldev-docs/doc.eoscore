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
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="/Script/OnlineSubsystemEOSCore.NetDriverEOSCore",DriverClassNameFallback="/Script/OnlineSubsystemUtils.IpNetDriver")

[EOSSDK]
bHasProjectBinary=true
```

This configuration makes EOSCore the default Online Subsystem and routes game traffic through EOS P2P (NAT traversal and relay servers) using the EOSCore NetDriver. The `[EOSSDK] bHasProjectBinary=true` entry makes the engine's own EOS modules use the EOS SDK binary that ships with EOSCore, so the two never load different SDK versions.

Your EOS credentials (ProductId, SandboxId, DeploymentId, ClientId, ClientSecret, EncryptionKey) are set in **Project Settings > Plugins > EOSCore Plugin**, see [Project Settings Reference](../settings_reference.md).

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