# Project Settings Reference

EOSCore is configured in **Project Settings > Plugins > EOSCore Plugin**. The settings are saved in `Config/DefaultEngine.ini` under the section `[/Script/OnlineSubsystemEOSCore.EOSCoreSettings]`.

## EOS Configurations
You can add several configurations, for example one for the client and one for a dedicated server. The plugin picks one when it starts:

1. The configuration named by the `-EOSConfig=` command line argument, if it exists.
2. Otherwise the configuration named by **Default Configuration Name**.

If no configuration matches, the plugin does not initialize and the log shows `GetEOSConfig(): Did not find any EOS Configuration`.

| Setting | Description |
|---|---|
| **Config Name** | Name used by Default Configuration Name and `-EOSConfig=` |
| **Product Id** | From the Developer Portal, Product Settings |
| **Sandbox Id** | From the Developer Portal. Overridden by `-epicsandboxid=` |
| **Deployment Id** | From the Developer Portal. Overridden by `-epicdeploymentid=` |
| **Client Id** | Client created in [Client & Policy Settings](./getting_started/setup_client_policy.md). Client ids start with `xyz` |
| **Client Secret** | Secret of that client |
| **Encryption Key** | Exactly 64 hexadecimal characters (a 256 bit key). Used to encrypt Player Data Storage files. The default value `1111…` is only a placeholder, generate your own key before you ship |
| **Enable RTC** | Enables the RTC (voice) interfaces. Turn it off if you don't use EOS voice |

:::warning
The Client Secret is stored in DefaultEngine.ini and ships with your game. Use a client policy that only allows what the game client needs, and a separate configuration with its own client for servers.
:::

## General Settings

| Setting | ini key | Description |
|---|---|---|
| **Default Configuration Name** | `DefaultConfigurationName` | Configuration used when `-EOSConfig=` is not set. Default `DefaultConfig` |
| **Tick Budget In Milliseconds** | `TickBudgetInMilliseconds` | Time budget for EOS work per tick. `0` means no limit |
| **Scope Flags** | `ScopeFlags` | Epic Account permissions requested by the **Login** node. Must match the permissions of your application in the Developer Portal, see [Scope Consent](./getting_started/setup_client_policy.md#scope-consent) |
| **iOS Client Id** | `IOSClientId` | Client Id used for the iOS login redirect, see [iOS Setup](./mobile/ios-setup.md) |
| **Android Client Id** | `AndroidClientId` | Client Id used for the Android login redirect, see [Android Setup](./mobile/android-setup.md) |
| **Disable EOS Overlay** | `bDisableEOSOverlay` | Disables the EOS overlay. Same as the `-DisableEOSOverlay` command line argument |

## ini-only Settings
These keys are not shown in Project Settings. Add them to the same section in DefaultEngine.ini.

```ini
[/Script/OnlineSubsystemEOSCore.EOSCoreSettings]
; Set to false to disable the EOSCore Online Subsystem
bEnabled=true
; Folder the EOS SDK uses for its cache (ignored on Android)
OverrideCacheDir=D:/MyGameCache
```

| ini key | Default | Description |
|---|---|---|
| `bEnabled` | `true` | Enables the EOSCore Online Subsystem. The older sections `[/Script/EOSCore.EOSCoreSettings]` and `[EOSCore]` are still read if this key is missing |
| `OverrideCacheDir` | User folder `/CacheDir` | Cache folder for the EOS SDK. Android always uses the app's internal files folder |

## Related Settings in Other Sections

| Section | Key | Description |
|---|---|---|
| `[OnlineSubsystem]` | `DefaultPlatformService=EOSCore` | Makes EOSCore the default Online Subsystem, see [Configuring Online Subsystem](./getting_started/configuring_subsystem.md) |
| `[EOSSDK]` | `bHasProjectBinary=true` | Makes the engine's EOS modules use the EOS SDK binary that ships with EOSCore |
| `[Voice]` | `bEnabled=true` | Needed for [Voice Chat (OSS)](./multiplayer/voice-chat/voice-chat_oss.md) |
