---
id: creating-multiplayer-session
title: Creating a Multiplayer Session
description: Learn how to create a multiplayer session using the EOSCore plugin with custom attributes and session name.
slug: /docfiles/eoscore/creating-multiplayer-session
---


# Creating a Multiplayer Session with EOSCore

This guide explains how to create a **multiplayer session** in Unreal Engine using the **EOSCore** plugin. The custom **Create EOS Session** node allows you to define a **session name** and **custom attributes**, giving you full control over session metadata. After creation, you must open your level with the `listen` parameter to accept incoming client connections.

:::note
  A **Video Tutorial** is available: [Multiplayer Sessions with EOSCore](../videos/multiplayer-sessions.mdx)
:::

---

## Prerequisites
- **EOSCore** plugin installed and enabled.
- A multiplayer map ready to host the session.

---

## Using the Create EOS Session Node

The **Create EOS Session** node is a custom, user-friendly wrapper that simplifies session creation with enhanced options.

### Key Features
- **Session Name**: Set a human-readable name (e.g., `"Team Deathmatch - Dust2"`).
- **Custom Attributes**: Add key-value pairs (e.g., `MapName`, `GameMode`, `MaxPlayers`) for filtering in server browsers.

---

### Step-by-Step Workflow

1. **Add the Node**:
   - In your **Game Instance**, **Game Mode**, or **Menu Blueprint**, add the **Create EOS Session** node.

2. **Configure Session Settings**:
   - **Session Name**: `"My Awesome Match"`
   - **Max Players**: `16`

3. **Open Level with Listen**:
   - On **Success**, use the **Open Level** node.
   - Set the level name and option to `listen`

   ![Create EOS Session and Open Level Example](../../../static/img/create_session.png)