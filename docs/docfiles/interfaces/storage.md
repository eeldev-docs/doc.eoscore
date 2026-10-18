---
sidebar_position: 7
---

# Title Storage and Player Data Storage

EOS has two file services:

| | Title Storage | Player Data Storage |
|---|---|---|
| Who writes | You, in the Developer Portal | The player, from the game |
| Who reads | All players | Only that player |
| Typical use | Patch notes, configuration, news | Save games, settings |

Both encrypt files with the **Encryption Key** of your configuration, see [Project Settings](../settings_reference.md). Changing the key makes existing files unreadable.

- [EOS Title Storage documentation](https://dev.epicgames.com/docs/epic-online-services/player-and-game-data/title-storage-interface)
- [EOS Player Data Storage documentation](https://dev.epicgames.com/docs/epic-online-services/player-and-game-data/player-data-storage-interface)

## Title Storage

| Node | Use |
|---|---|
| `EOS_TitleStorage_QueryFileList` | Lists files with the given tags |
| `EOS_TitleStorage_GetFileMetadataCount` + `EOS_TitleStorage_CopyFileMetadataAtIndex` | Reads file names, sizes and hashes |
| `EOS_TitleStorage_QueryFile` / `EOS_TitleStorage_CopyFileMetadataByFilename` | Metadata of one file |
| `EOS_TitleStorage_ReadFile` | Downloads a file to a local file, with progress |
| `EOS_TitleStorage_DeleteCache` | Clears the local cache |

Files are uploaded in the Developer Portal under **Game Services > Title Storage**, with tags used by QueryFileList.

## Player Data Storage
See the step by step [SaveGames example](../examples/savegames/creating_savegame.md).

- Use **WriteFile** to upload a local file and **ReadFile** to download it. Use an absolute path for the local file.
- Run one read and one write at a time. Wait for the completion callback before you start the next transfer of the same kind.
- Files are limited in size and count per player, see the EOS documentation for the current limits.
