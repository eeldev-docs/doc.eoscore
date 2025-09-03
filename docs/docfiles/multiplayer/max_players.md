---
id: configuring-maxplayers
title: Max Players for Game Sessions
description: Learn how to configure the maximum number of players for your Unreal Engine game session using the GameSession settings.
slug: /docfiles/game-session/maxplayers

---

# Configuring Max Players for Game Sessions

This guide explains how to set or override the maximum number of players (`MaxPlayers`) for your Unreal Engine game session using the **GameSession** settings in your configuration files.

## Default Configuration
The default value for `MaxPlayers` is defined in the `Engine\Config\BaseGame.ini` file:

```ini
[/Script/Engine.GameSession]
MaxPlayers=16
```

This sets the default maximum number of players to 16 for a game session unless overridden.

## Overriding the Default Value
To customize the `MaxPlayers` value for your game, you can override the default setting by adding or modifying the following in your project’s `Config\DefaultGame.ini` file:

```ini
[/Script/Engine.GameSession]
MaxPlayers=64
```

### Steps
1. **Open DefaultGame.ini**:
   - Locate your project’s `DefaultGame.ini` file in the `Config` directory (e.g., `YourProject/Config/DefaultGame.ini`).

2. **Add or Update the Setting**:
   - Under the `[/Script/Engine.GameSession]` section, set `MaxPlayers` to your desired value (e.g., `64`).
   - If the section doesn’t exist, create it and add the line.

3. **Save and Apply**:
   - Save the `DefaultGame.ini` file.
   - Restart the Unreal Editor or rebuild your project to apply the changes.

## Notes
- **Scope**: The `MaxPlayers` setting applies to the game session and affects the maximum number of players that can join a multiplayer session or server.