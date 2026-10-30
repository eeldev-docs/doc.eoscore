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

## Steam Integrated Platform
Registers Steam as an [Integrated Platform](./interfaces/integrated-platform.md) when the EOS platform is created (Windows, Linux and Mac). EOS then mirrors presence and sessions to Steam and follows the Steam login state.

| Setting | ini key | Description |
|---|---|---|
| **Enable Steam Integrated Platform** | `bEnableSteamIntegratedPlatform` | Turns the integration on. Default `false` |
| **Steam Major Version** | `SteamMajorVersion` | Major version of the Steamworks SDK the game is built with. Default `1` |
| **Steam Minor Version** | `SteamMinorVersion` | Minor version of the Steamworks SDK, for example `64` for UE 5.8 (Steamworks 1.64). Must be set, the integration is skipped while it is `0` |
| **Steam Disable Presence Mirroring** | `bSteamDisablePresenceMirroring` | Stops EOS from copying presence to Steam |
| **Steam Disable SDK Managed Sessions** | `bSteamDisableSDKManagedSessions` | Stops EOS from creating Steam sessions for EOS sessions and lobbies |
| **Steam Prefer Integrated Identity** | `bSteamPreferIntegratedIdentity` | Shows Steam names instead of Epic names where both exist |

The Steam library is managed by the game, so the Steam Online Subsystem (or your own code) must initialize Steam before EOSCore creates the platform.

## ini-only Settings
These keys are not shown in Project Settings. Add them to the same section in DefaultEngine.ini.

```ini
[/Script/OnlineSubsystemEOSCore.EOSCoreSettings]
; Set to false to disable the EOSCore Online Subsystem
bEnabled=true
; Folder the EOS SDK uses for its cache (ignored on Android)
OverrideCacheDir=D:/MyGameCache
; Log level of the EOS SDK's own messages (non-Shipping builds only)
SDKLogLevel=Warning
; Seconds EOS retries network tasks before they fail
TaskNetworkTimeoutSeconds=10
```

| ini key | Default | Description |
|---|---|---|
| `bEnabled` | `true` | Enables the EOSCore Online Subsystem. The older sections `[/Script/EOSCore.EOSCoreSettings]` and `[EOSCore]` are still read if this key is missing |
| `OverrideCacheDir` | User folder `/CacheDir` | Cache folder for the EOS SDK. Android always uses the app's internal files folder |
| `SDKLogLevel` | `VeryVerbose` | `Off`, `Fatal`, `Error`, `Warning`, `Info`, `Verbose` or `VeryVerbose`. Lower it if the EOS SDK messages flood your log. Not used in Shipping builds, where SDK logging is off |
| `TaskNetworkTimeoutSeconds` | `10` | How long EOS retries a network task (for example a login while offline) before the callback fails |

## Related Settings in Other Sections

| Section | Key | Description |
|---|---|---|
| `[OnlineSubsystem]` | `DefaultPlatformService=EOSCore` | Makes EOSCore the default Online Subsystem, see [Configuring Online Subsystem](./getting_started/configuring_subsystem.md) |
| `[EOSSDK]` | `bHasProjectBinary=true` | Makes the engine's EOS modules use the EOS SDK binary that ships with EOSCore |
| `[Voice]` | `bEnabled=true` | Needed for [Voice Chat (OSS)](./multiplayer/voice-chat/voice-chat_oss.md) |
