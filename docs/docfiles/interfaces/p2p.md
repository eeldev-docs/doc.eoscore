---
sidebar_position: 5
---

# P2P

The P2P interface sends packets directly between players, with NAT traversal and Epic relay servers when a direct connection is not possible. Players are addressed by their **Product User Id**, no IP addresses or open ports are needed.

- [EOS P2P documentation](https://dev.epicgames.com/docs/game-services/p-2-p)

:::tip
For normal Unreal replication you don't need these nodes. The EOSCore NetDriver uses P2P for you, see [Configuring Online Subsystem](../getting_started/configuring_subsystem.md). Use the P2P nodes for your own messages, for example chat or a lobby handshake.
:::

## Common Nodes

| Node | Use |
|---|---|
| `EOS_P2P_SendPacket` | Sends data to a remote user on a socket name and channel. The first packet opens the connection |
| `EOS_P2P_AddNotifyPeerConnectionRequest` | Called when a remote user wants to connect |
| `EOS_P2P_AcceptConnection` | Accepts the connection (call it for incoming requests, or before you send to open both ways) |
| `EOS_P2P_GetNextReceivedPacketSize` + `EOS_P2P_ReceivePacket` | Reads received packets, call it every tick |
| `EOS_P2P_CloseConnection` / `EOS_P2P_CloseConnections` | Closes one or all connections on a socket |
| `EOS_P2P_AddNotifyPeerConnectionEstablished` / `Interrupted` / `Closed` | Connection state changes |

## Settings

| Node | Use |
|---|---|
| `EOS_P2P_QueryNATType` + `EOS_P2P_GetNATType` | Finds the local NAT type |
| `EOS_P2P_SetRelayControl` / `EOS_P2P_GetRelayControl` | Allows, forces or forbids relay servers. Forcing relays hides player IP addresses from each other |
| `EOS_P2P_SetPortRange` / `EOS_P2P_GetPortRange` | Limits the local UDP ports |
| `EOS_P2P_SetPacketQueueSize` + `EOS_P2P_AddNotifyIncomingPacketQueueFull` | Limits the packet queues |
