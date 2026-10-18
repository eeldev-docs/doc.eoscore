---
sidebar_position: 1
---

# Stats

:::info Video Tutorial is available
[Stats video](../videos/stats.mdx)
:::

Stats are numeric values per player (kills, wins, playtime) that EOS aggregates on the backend. Define them in the Developer Portal (**Game Services > Stats**) with an aggregation type (SUM, LATEST, MIN or MAX). [Leaderboards](./leaderboards.md) and stat based [Achievements](./achievements.md) are built on stats.

- [EOS Stats documentation](https://dev.epicgames.com/docs/game-services/eos-stats-interface)

## Requirements
- A logged in user with a **Product User Id** (Connect login)
- The **Stats** feature allowed in your client policy

## Common Nodes

| Node | Use |
|---|---|
| `EOS_Stats_IngestStat` | Sends one or more stat values. The backend aggregates them with the stat's aggregation type |
| `EOS_Stats_QueryStats` | Downloads the stats of a player |
| `EOS_Stats_GetStatsCount` + `EOS_Stats_CopyStatByIndex` | Reads the downloaded stats |
| `EOS_Stats_CopyStatByName` | Reads one downloaded stat by name |

## Notes
- Ingested values are added to the aggregation, they don't replace the stat (except LATEST). Ingest the change (for example `Kills = 1`), not the total.
- Stats are not updated immediately. Query them again after a short delay if you need the new value.

## Online Subsystem
The `EOSCore` Online Subsystem also implements `IOnlineStats` (`UpdateStats`, `QueryStats`).
