---
id: lobby-sessions
title: Lobby Sessions (Online Subsystem)
description: Use EOS Lobbies through the Online Subsystem session interface with bUseLobbiesIfAvailable and SEARCH_LOBBIES.
---

# Lobby Sessions (Online Subsystem)

The EOSCore Online Subsystem can use **EOS Lobbies** instead of EOS Sessions behind the normal `IOnlineSession` interface. The same Create, Find, Join, Update, Destroy and invite functions work. They talk to the EOS Lobby interface when the session settings ask for it.

Lobby sessions are meant for player hosted games, such as parties, listen servers and peer-to-peer games. Dedicated servers cannot own a lobby, so use EOS Sessions for them.

## Requirements
- The local user is logged in with EOS Connect (a Product User Id).
- Players connect to the lobby owner over EOS P2P, so use the EOSCore NetDriver with `bIsUsingP2PSockets=true`. See [Configuring Online Subsystem](../getting_started/configuring_subsystem.md).
- Create the session **before** you open the level with `listen`. The host listens on a P2P socket named after the lobby id.

## Creating a Lobby Session
Set `bUseLobbiesIfAvailable` in the session settings.

```cpp
FOnlineSessionSettings Settings;
Settings.NumPublicConnections = 4;
Settings.bShouldAdvertise = true;
Settings.bUsesPresence = true;
Settings.bAllowJoinViaPresence = true;
Settings.bAllowInvites = true;
Settings.bUseLobbiesIfAvailable = true;
Settings.bUseLobbiesVoiceChatIfAvailable = false;
Settings.Set(SETTING_HOST_MIGRATION, true, EOnlineDataAdvertisementType::DontAdvertise);
Settings.Set(FName(TEXT("GAMEMODE")), FString(TEXT("Coop")), EOnlineDataAdvertisementType::ViaOnlineService);

SessionInterface->CreateSession(0, NAME_GameSession, Settings);
```

| Setting | Lobby option |
|---|---|
| `NumPublicConnections` + `NumPrivateConnections` | Max lobby members (EOS allows up to 64) |
| `NumPublicConnections > 0` | Public advertised lobby |
| `NumPublicConnections == 0` and `bAllowJoinViaPresence` | Join via presence |
| Otherwise | Invite only |
| `bUsesPresence` | Lobby presence, so friends can join from the EOS overlay |
| `bAllowInvites` | Invites allowed |
| `bUseLobbiesVoiceChatIfAvailable` | Creates the lobby RTC voice room. The platform must be created with RTC enabled |
| `SETTING_HOST_MIGRATION` | Host migration, on by default |
| `SessionIdOverride` (UE 5.1+) | Custom lobby id, 4 to 32 characters |

Settings advertised with `ViaOnlineService` or `ViaOnlineServiceAndPing` become lobby attributes. `MemberSettings` for the local user become member attributes.

## Finding Lobby Sessions
Set `SEARCH_LOBBIES` to `true` in the query settings. The other query settings are used as lobby search filters.

```cpp
SessionSearch = MakeShared<FOnlineSessionSearch>();
SessionSearch->MaxSearchResults = 50;
SessionSearch->QuerySettings.Set(SEARCH_LOBBIES, true, EOnlineComparisonOp::Equals);
SessionSearch->QuerySettings.Set(FName(TEXT("GAMEMODE")), FString(TEXT("Coop")), EOnlineComparisonOp::Equals);

SessionInterface->FindSessions(0, SessionSearch.ToSharedRef());
```

`FindFriendSession` finds the lobby a friend is in, and `FindSessionById` looks for a lobby with that id first and then for an EOS session.

## Joining and Connecting
Join a search result with `JoinSession`. When `OnJoinSessionComplete` reports success, get the connect string and travel:

```cpp
FString ConnectString;
if (SessionInterface->GetResolvedConnectString(NAME_GameSession, ConnectString))
{
	PlayerController->ClientTravel(ConnectString, TRAVEL_Absolute);
}
```

The connect string is an EOS P2P address for the lobby owner, `EOS:<OwnerProductUserId>:<LobbyId>:<Channel>`.

## Updates, Members and Invites
- `UpdateSession` updates the lobby. Only the owner can change lobby settings and attributes. Other members only update their own member attributes.
- Lobby changes call `OnSessionSettingsUpdated`. Members joining and leaving call `OnSessionParticipantJoined` and `OnSessionParticipantLeft` (UE 5.2+) or `OnSessionParticipantsChange` (older engines). Member attribute changes call `OnSessionParticipantSettingsUpdated`.
- `SendSessionInviteToFriend` sends a lobby invite. Received invites call `OnSessionInviteReceived`. Accepted invites and **Join** in the EOS overlay call `OnSessionUserInviteAccepted`.
- **Leave** in the EOS overlay calls `OnDestroySessionRequested` (UE 5.5+). On older engines the session is destroyed directly.
- `DestroySession` leaves the lobby. If the owner leaves with host migration on, EOS promotes another member, and that member's session becomes the owner. If the lobby is closed or the local user is kicked, the session is removed and `OnDestroySessionComplete` is called.
