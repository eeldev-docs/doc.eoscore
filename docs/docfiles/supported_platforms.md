# Supported Platforms and Features

## Versions

| | Version |
|---|---|
| EOSCore | 2.0.0 |
| EOS SDK (included) | 1.19.2 (hotfix 1) |

For the engine versions of the current release, see the [Fab listing](https://www.fab.com/listings/8e78818d-4662-4c6d-8ab2-6e22b9d3595e). Older engine versions are available on [Gumroad](https://eeldev.gumroad.com/l/eoscore).

## Platforms

| Platform | Supported | Notes |
|---|---|---|
| Windows (x64) | Yes | |
| Linux (x64) | Yes | Also for dedicated servers |
| Mac | Yes | |
| Android | Yes | arm64-v8a, armeabi-v7a and x86_64. See [Android Setup](./mobile/android-setup.md) |
| iOS | Yes | See [iOS Setup](./mobile/ios-setup.md) |
| Windows ARM64, Linux ARM64 | No | |
| Consoles | No | |

## Plugin Modules

| Module | Description |
|---|---|
| **EOSCore** | Blueprint functions and async nodes for the EOS SDK interfaces (Auth, Connect, Sessions, Lobby, P2P, Stats, Leaderboards, Achievements, Player Data Storage, Title Storage, Ecom, Presence, Friends, UserInfo, UI, RTC, Chat, Anti-Cheat, Sanctions, Reports, Mods, Metrics, Custom Invites, Progression Snapshot) |
| **OnlineSubsystemEOSCore** | The `EOSCore` Online Subsystem, NetDriver and socket subsystem for EOS P2P |
| **EOSCoreWeb** | Blueprint functions for the EOS Web API (Auth, Connect, Player Tickets, Sanctions, Anti-Cheat status, Voice), see [Web API](./web/introduction.md) |
| **EOSCoreShared** | Types shared between the modules |

## Online Subsystem Interfaces
These are the `IOnlineSubsystem` interfaces the EOSCore Online Subsystem implements. Interfaces marked No return `nullptr`, use the EOSCore Blueprint functions for those features instead.

| Interface | Implemented | Notes |
|---|---|---|
| Identity | Yes | Login, logout, automatic login with [-AUTH_TYPE](./commandline_arguments.md) |
| Session | Yes | EOS Sessions, EOS Lobbies (`bUseLobbiesIfAvailable`, see [Lobby Sessions](./multiplayer/lobby-sessions.md)) and LAN sessions. **Join** in the EOS overlay and accepted invites call `OnSessionUserInviteAccepted` |
| Stats | Yes | |
| Leaderboards | Yes | |
| Achievements | Yes | |
| Voice | Yes | See [Voice Chat (OSS)](./multiplayer/voice-chat/voice-chat_oss.md) |
| Store V2 / Purchase | Only when started from the Epic Games Launcher (`-EpicPortal`) | |
| Friends | Yes | Friends list, friend invites, blocked players. Needs an Epic Account login |
| Presence | Yes | Set and query presence, presence change notifications. Needs an Epic Account login |
| User | Yes | User info (display names) and external account id mappings |
| External UI | Partly | `ShowLoginUI` (Account Portal), `ShowFriendsUI` (EOS overlay) and `ShowWebURL`. The other Show functions are not implemented |
| User Cloud, Shared Cloud, Title File, Entitlements | No | Use Player Data Storage, Title Storage and Ecom functions |
| Party, Groups, Chat, Messages, Events, Sharing, Time, Tournaments, Turn Based | No | |
