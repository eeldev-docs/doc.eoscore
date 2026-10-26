# Changelog

## 1.9.8.9

### New

- **Online Subsystem interfaces.** The `EOSCore` Online Subsystem now implements `IOnlineFriends`, `IOnlinePresence`, `IOnlineUser` and parts of `IOnlineExternalUI`. See [Supported Platforms and Features](./docfiles/supported_platforms.md#online-subsystem-interfaces).
  - **Friends:** read the friends list, send, accept and reject friend invites, query blocked players, and get friends list and blocklist notifications.
  - **Presence:** set and query presence, with presence change notifications. Friend presence is kept in sync.
  - **User:** query user info and display names, look up users by display name, and query external account mappings.
  - **External UI:** `ShowLoginUI` (Account Portal), `ShowFriendsUI` (EOS overlay), `ShowWebURL` and `OnExternalUIChange`.
- **Join from the overlay.** Pressing **Join** on a friend in the EOS social overlay now calls `OnSessionUserInviteAccepted` with the friend's session. See [Join Friends Multiplayer Game](./docfiles/multiplayer/friends/join-friend-session.md).
- **Lobby sessions.** With `bUseLobbiesIfAvailable` the Online Subsystem session interface creates, finds (`SEARCH_LOBBIES`), joins, updates and leaves EOS Lobbies, with lobby invites, overlay joins, member attributes and participant joined/left notifications. See [Lobby Sessions](./docfiles/multiplayer/lobby-sessions.md).
- **`FindSessionById`** finds lobbies and EOS sessions by id, and **`FindFriendSession`** finds the lobby a friend is in.
- **`UpdateSession`** now applies changes to `NumPublicConnections` (max players) and `bAllowInvites`.
- **Platform handle provider.** The EOS platform is shared through the `EOSPlatformHandleProvider` modular feature, so other plugins (such as HostMigrationCore) can use the same EOS platform without depending on EOSCore. See [Platform Handle Provider](./docfiles/platform_handle_provider.md).
- **New settings** in the `[/Script/OnlineSubsystemEOSCore.EOSCoreSettings]` section:
  - `SDKLogLevel` sets the EOS SDK log level.
  - `TaskNetworkTimeoutSeconds` sets the network timeout for EOS tasks.

  See [Settings Reference](./docfiles/settings_reference.md).
- **Connect logout.** Logging out of the Online Subsystem now also calls `EOS_Connect_Logout`.
- **Mobile lifecycle.** The EOS application status is set to background suspended or foreground when the app is suspended or resumed.

### Changed

- **Platforms:** the plugin now lists its supported platforms (`PlatformAllowList`), so it is no longer loaded on unsupported platforms.
- **`-AUTH_TYPE` automatic login:** credential types now have to match the EOS SDK type names exactly (not case sensitive). An unknown type fails the login instead of picking the wrong credential type. See [Command Line Arguments](./docfiles/commandline_arguments.md).
- **Blueprint functions** called before EOS is initialized, or without a valid platform, now return `EOS_NotConfigured` instead of failing without a result.
- **Async nodes** called without a valid platform now always run their failure output instead of never completing.
- **Multiple worlds:** when a world is available (PIE with several clients, for example), the EOSCore subsystem now only uses that world's instance.
- **EOS log messages** (`ListenForEOSMessages`) are now delivered on the game thread. SDK fatal messages are logged as `Error`, so they no longer crash the game.
- **Configuration overrides:** `DefaultConfigurationName` is only used when no other configuration matches.
- **Settings:** `bEnabled` is now read from the `OnlineSubsystemEOSCore` settings section.
- **Logging:**
  - Access tokens, auth tokens and Web API responses are no longer written to the log.
  - A missing Android or iOS Client Id now gives a build warning.
- **Android:** the default `AndroidClientId` is now empty instead of an example value.
- **Packaging:** `xaudio2_9redist.dll` is staged with the EOS SDK, and the bundled copy is used when the system copy is missing.
- **Error dialog:** the dialog for a failed EOS SDK load is no longer shown on dedicated servers, in commandlets or in unattended runs.

### Fixed

- Crash when Online Subsystem callbacks ran after their interface had been destroyed, for example during shutdown or a level change.
- EOS notifications not removed when the Identity and Session interfaces were destroyed.
- Null dereference in `EndSession` for LAN sessions.
- Null dereferences in the Auth and Connect login status changed handlers.
- `Login` continuing when the EOS subsystem was missing.
- Memory leaks in the session invite received and invite accepted handlers. Accepting an invite also checks that the session details could be read.
- PlayerDataStorage reads and writes sharing the same progress callback.
- PlayerDataStorage transfer handles that were never released.
- The EOS platform not being released on shutdown, and the SDK not being shut down last.
- Wrong EOS cache directory on iOS.
- A heap allocation for the XAudio DLL path that was never freed.
- Async action callback signatures that did not match their delegates.
- Compatibility with newer engine versions in the Build.cs version checks.
- `OnJoinSessionComplete` being called twice when joining an EOS session, and a failed join leaving the session behind.
- The completion delegate passed to `DestroySession` not being called for EOS sessions.
- Session invites failing for friends whose Product User Id was not cached by the Identity interface.
- `SEARCH_LOBBIES` being sent as a search filter in EOS session searches.
