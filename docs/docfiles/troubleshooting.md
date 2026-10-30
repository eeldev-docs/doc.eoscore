# Troubleshooting

Start by turning on [Additional Logging](./additional_logging.md) and read the log in your project's `Saved/Logs` folder. Most problems show up there as an EOS result code.

## Common Result Codes

| Result | Usual cause | Fix |
|---|---|---|
| `EOS_NotConfigured` | The plugin did not create an EOS platform, so every EOSCore function and async node fails right away with this result | Check that your configuration in [Project Settings](./settings_reference.md) is complete and that its name matches **Default Configuration Name** (or `-EOSConfig=`). Look for `GetEOSConfig()` and `PlatformCreate()` errors in the log |
| `EOS_InvalidUser` | Connect login worked, but there is no EOS user for this external account yet | Call **EOS_Connect_CreateUser** with the `ContinuanceToken` from the login callback (once per account). The **Login** node does this for you |
| `EOS_DuplicateNotAllowed` from CreateDeviceId | A DeviceId already exists on this device | Not an error, continue with the login |
| `EOS_AccessDenied`, `EOS_InvalidAuth` | The client policy does not allow this action, or ClientId/ClientSecret are wrong | Check the client and its policy in the Developer Portal, see [Client & Policy Settings](./getting_started/setup_client_policy.md) |
| `EOS_InvalidParameters` | A required option is empty, often `LocalUserId` because the user is not logged in yet | Log in first, see [Authentication](./getting_started/auth/information.md) |
| `EOS_Auth_ScopeConsentRequired` and other scope errors | The Scope Flags in Project Settings do not match the application permissions | Make them match, see [Scope Consent](./getting_started/setup_client_policy.md#scope-consent) |

## The plugin does not start

- **"Failed to load EOS third party library"**: the EOS SDK binary next to the plugin could not be loaded. Check that `Plugins/EOSCore/Source/ThirdParty/EOSLibrary/Bin` exists in your build and that no other EOS SDK version is loaded. The **EOS_GetVersion** node returns the version of the EOS SDK that was loaded, so you can check it in a packaged build.
- **Warnings about EOSShared, OnlineSubsystemEOS, EOSVoiceChat or SocketSubsystemEOS** in the editor log: one of the engine's EOS plugins is enabled. Disable them, or add `[EOSSDK] bHasProjectBinary=true`, see [Installing the Plugin](./getting_started/installing_plugin.md).
- **Nothing happens at all**: check that `[OnlineSubsystem] DefaultPlatformService=EOSCore` is set and that `bEnabled` is not `false`, see [Configuring Online Subsystem](./getting_started/configuring_subsystem.md).

## Login problems

- **The Epic Account login (Account Portal) does not open**: the EOS overlay only works in a Standalone Game or a packaged game, not in PIE. See [Epic Account](./getting_started/auth/auth_epic.md).
- **Login works but Online Subsystem functions say the player is not logged in**: when you log in with the async nodes you have to update the player's UniqueNetId yourself, see [Authentication](./getting_started/auth/information.md).
- **The player is logged out after about an hour**: login tokens expire, see [Refreshing Tokens](./getting_started/auth/refreshing_tokens.md).

## Multiplayer problems

- **Two instances on the same computer can't see each other**: both use the same DeviceId. Use two different accounts, for example two DevTool credentials, see [Testing Multiplayer](./multiplayer/testing-multiplayer.md).
- **Can't join a session after leaving one**: the old session was not destroyed, see [Leaving a Session](./multiplayer/leave-session.md).
- **Can't join your own dedicated server from the server browser**: the server reports its external IP. Connect with `open <local ip>` on your local network, see [Dedicated Server](./multiplayer/dedicated-server.md).

## Player Data Storage

- **ReadFile/WriteFile with a relative path fails**: use an absolute path for the local file.
- Run one read and one write at a time. Starting a second transfer of the same kind before the first one has finished is not supported.
