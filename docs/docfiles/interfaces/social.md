---
sidebar_position: 9
---

# Friends, Presence and User Info

These interfaces use **Epic Account Services** and need an Epic Account login (Auth interface) with the matching permissions (scopes), see [Scope Consent](../getting_started/setup_client_policy.md#scope-consent).

- [EOS Friends documentation](https://dev.epicgames.com/docs/epic-online-services/accounts-and-social/eos-friends-interface)
- [EOS Presence documentation](https://dev.epicgames.com/docs/epic-online-services/accounts-and-social/eos-presence-interface)
- [EOS User Info documentation](https://dev.epicgames.com/docs/epic-online-services/accounts-and-social/eos-user-info-interface)

:::note
The `EOSCore` Online Subsystem also implements `IOnlineFriends`, `IOnlinePresence`, `IOnlineUser` and parts of `IOnlineExternalUI` (login, friends overlay, web URL), so C++ code and plugins that use the Online Subsystem can use them as well. In Blueprints, use the EOSCore nodes below. See [Supported Platforms and Features](../supported_platforms.md#online-subsystem-interfaces).
:::

## Friends

| Node | Use |
|---|---|
| `EOS_Friends_QueryFriends` | Downloads the friends list (needs the Friends List scope) |
| `EOS_Friends_GetFriendsCount` + `EOS_Friends_GetFriendAtIndex` | Reads the friends' Epic Account Ids |
| `EOS_Friends_GetStatus` | Friend, invite sent, invite received or not friends |
| `EOS_Friends_SendInvite`, `EOS_Friends_AcceptInvite`, `EOS_Friends_RejectInvite` | Friend invites |
| `EOS_Friends_AddNotifyFriendsUpdate` | Called when the friends list changes |

## Presence

| Node | Use |
|---|---|
| `EOS_Presence_CreatePresenceModification` | Creates a modification handle |
| `EOS_PresenceModification_SetStatus`, `SetRawRichText`, `SetData`, `SetJoinInfo` | Online status, rich text ("In a match"), key/value data and the join info friends use to join you |
| `EOS_Presence_SetPresence` | Sends the modification, then `EOS_PresenceModification_Release` |
| `EOS_Presence_QueryPresence` + `EOS_Presence_CopyPresence` | Reads a friend's presence |
| `EOS_Presence_AddNotifyOnPresenceChanged` | Called when a friend's presence changes |
| `EOS_Presence_AddNotifyJoinGameAccepted` | The player pressed **Join** on a friend in the social overlay, see [Join Friends Game](../multiplayer/friends/join-friend-session.md) |

## User Info

| Node | Use |
|---|---|
| `EOS_UserInfo_QueryUserInfo` | Downloads a user's account info |
| `EOS_UserInfo_CopyUserInfo` | Display name, country, language |
| `EOS_UserInfo_CopyBestDisplayName` | The name to show for a player, also for players from other platforms |
| `EOS_UserInfo_QueryUserInfoByDisplayName` | Finds a user by display name |

See [Player Name](../examples/player_name.md) for an example.
