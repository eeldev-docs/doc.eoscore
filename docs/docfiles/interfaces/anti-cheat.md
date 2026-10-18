---
sidebar_position: 6
---

# Anti-Cheat

:::info Video Tutorials are available
[Cheaters video](../videos/cheaters.mdx) and [Reporting Cheaters video](../videos/reporting-cheaters.mdx)
:::

EOS Anti-Cheat protects the game client and lets the server check that clients are protected. The game must be started through the Anti-Cheat launcher with a protected build, see the EOS documentation for how to set this up.

- [EOS Anti-Cheat documentation](https://dev.epicgames.com/docs/game-services/anti-cheat)

:::warning
Anti-Cheat needs an external account. A DeviceId login is not enough, see [DeviceId](../getting_started/auth/auth_deviceid.md).
:::

## Client-Server Mode
Use this mode with a dedicated or listen server.

**Server** (AntiCheatServer nodes):
1. `EOS_AntiCheatServer_AddNotifyMessageToClient`: send the message to that client over your own network channel (for example a reliable RPC)
2. `EOS_AntiCheatServer_AddNotifyClientActionRequired`: kick the client when EOS asks for it
3. `EOS_AntiCheatServer_BeginSession` when the server starts
4. `EOS_AntiCheatServer_RegisterClient` for each connecting player, `EOS_AntiCheatServer_UnregisterClient` when they leave
5. `EOS_AntiCheatServer_ReceiveMessageFromClient` for each anti-cheat message from a client
6. `EOS_AntiCheatServer_EndSession` when the server stops

**Client** (AntiCheatClient nodes):
1. `EOS_AntiCheatClient_AddNotifyMessageToServer`: send the message to the server over your own network channel
2. `EOS_AntiCheatClient_BeginSession` in client-server mode when you connect, `EOS_AntiCheatClient_EndSession` when you leave
3. `EOS_AntiCheatClient_ReceiveMessageFromServer` for each anti-cheat message from the server
4. `EOS_AntiCheatClient_AddNotifyClientIntegrityViolated`: the client is not protected

## Peer-to-Peer Mode
Without a trusted server, every client registers the other players with `EOS_AntiCheatClient_RegisterPeer`, and forwards messages with `EOS_AntiCheatClient_AddNotifyMessageToPeer` and `EOS_AntiCheatClient_ReceiveMessageFromPeer`.

## Gameplay Events
The server can send gameplay events to the Anti-Cheat backend for analysis, for example `EOS_AntiCheatServer_LogPlayerSpawn`, `EOS_AntiCheatServer_LogPlayerTakeDamage`, `EOS_AntiCheatServer_LogGameRoundStart` and `EOS_AntiCheatServer_LogGameRoundEnd`.

## Reports and Sanctions
Players can report each other with `EOS_Reports_SendPlayerBehaviorReport`. Use `EOS_Sanctions_QueryActivePlayerSanctions` to check if a player is banned, and the [EOSCore Web](../../docfiles-web/introduction.md) sanctions functions to create sanctions from a server or tool.
