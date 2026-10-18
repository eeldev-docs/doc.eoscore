---
sidebar_position: 4
---

# Lobby

A lobby is a group of players that share attributes and can talk with voice (RTC) before or during a match. Lobbies are separate from [game sessions](../multiplayer/creating-session.md): a common setup is to gather players in a lobby, then move them to a game session, see [Lobby to Session](../multiplayer/lobby-to-session.md).

- [EOS Lobbies documentation](https://dev.epicgames.com/docs/game-services/lobbies)

## Requirements
- A logged in user with a **Product User Id** (Connect login)
- The **Lobbies** feature allowed in your client policy
- **Enable RTC** in [Project Settings](../settings_reference.md) if you use lobby voice

## Creating and Joining

| Node | Use |
|---|---|
| `EOS_Lobby_CreateLobby` | Creates a lobby, you become the owner. Options include max members, permission level, presence, RTC room and host migration |
| `EOS_Lobby_CreateLobbySearch` | Creates a search handle |
| `EOS_LobbySearch_SetParameterString` / `Bool` / `Double`, `EOS_LobbySearch_SetLobbyId`, `EOS_LobbySearch_SetTargetUserId` | Search filters |
| `EOS_LobbySearch_Find` | Runs the search |
| `EOS_LobbySearch_GetSearchResultCount` + `EOS_LobbySearch_CopySearchResultByIndex` | Gets a lobby details handle per result |
| `EOS_Lobby_JoinLobby` / `EOS_Lobby_JoinLobbyById` | Joins with a details handle or by lobby id |
| `EOS_Lobby_LeaveLobby` / `EOS_Lobby_DestroyLobby` | Leaves, or (owner) destroys the lobby |

Release every details handle (`EOS_LobbyDetails_Release`) and search handle (`EOS_LobbySearch_Release`) when you are done with it.

## Attributes

| Node | Use |
|---|---|
| `EOS_Lobby_UpdateLobbyModification` | Creates a modification handle (owner for lobby attributes, any member for member attributes) |
| `EOS_LobbyModification_AddAttribute*`, `EOS_LobbyModification_AddMemberAttribute*`, `EOS_LobbyModification_RemoveAttribute` | Changes attributes |
| `EOS_LobbyModification_SetMaxMembers`, `SetPermissionLevel`, `SetInvitesAllowed`, `SetBucketId` | Changes lobby settings |
| `EOS_Lobby_UpdateLobby` | Sends the modification |
| `EOS_LobbyDetails_CopyAttributeByKey`, `EOS_LobbyDetails_CopyMemberAttributeByKey` | Reads attributes |

## Notifications
Bind these once, for example in the Game Instance:
- `EOS_Lobby_AddNotifyLobbyUpdateReceived`: lobby attributes changed
- `EOS_Lobby_AddNotifyLobbyMemberUpdateReceived`: a member's attributes changed
- `EOS_Lobby_AddNotifyLobbyMemberStatusReceived`: a member joined, left, was kicked or promoted, or the lobby closed
- `EOS_Lobby_AddNotifyLobbyInviteReceived` / `EOS_Lobby_AddNotifyLobbyInviteAccepted`: invites
- `EOS_Lobby_AddNotifyJoinLobbyAccepted`: the player pressed **Join** in the social overlay, see [Join Friends Lobby](../multiplayer/friends/join-friend-lobby.md)

## Owner Actions
`EOS_Lobby_KickMember`, `EOS_Lobby_PromoteMember` and `EOS_Lobby_HardMuteMember` are only allowed for the lobby owner. If host migration is enabled, EOS picks a new owner when the owner leaves.

## Voice
With **bEnableRTCRoom** set when creating the lobby, every member joins the lobby's voice room automatically. Use `EOS_Lobby_GetRTCRoomName` to get the room for the RTC Audio nodes (mute, volume) and `EOS_Lobby_AddNotifyRTCRoomConnectionChanged` to see when voice connects.
