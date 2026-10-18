# Commandline Arguments
There are several commandline arguments that the plugin can use

## Available Arguments

- **-AUTH_TYPE=xxx**<br />
Logs in the first local player automatically when the Online Subsystem starts, see the supported types below. Not used on dedicated server builds (`UE_SERVER`).
- **-AUTH_LOGIN=xxx**<br />
Login Id used by the automatic login, for example the DevTool host address
- **-AUTH_TOKEN=xxx**<br />
Token or password used by the automatic login. If it is not set, **-AUTH_PASSWORD=** is used instead (this is what the Epic Games Launcher passes)
- **-EOSConfig=configurationName**<br />
Use this configuration from Project Settings instead of the Default Configuration Name
- **-epicsandboxid=xxx**<br />
Overrides the SandboxId of the selected configuration (passed by the Epic Games Launcher)
- **-epicdeploymentid=xxx**<br />
Overrides the DeploymentId of the selected configuration
- **-EpicPortal**<br />
Tells the plugin the game was started by the Epic Games Launcher, this enables the Online Subsystem Store/Purchase interface
- **-DisableEOSOverlay**<br />
Disables the EOS overlay (same as `bDisableEOSOverlay` in Project Settings)
- **-Port=xxxx**<br />
Game port. The Online Subsystem writes it to the session as the `ServerPort` attribute so clients connect to the right port

## Supported AUTH_TYPE values
The value is not case sensitive. An unknown value makes the automatic login fail with an error in the log.

| AUTH_TYPE | Login method |
|---|---|
| `devtool` or `developer` | DevTool, AUTH_LOGIN is the DevTool host (`localhost:1111`), AUTH_TOKEN is the credential name |
| `persistentauth` or `autologin` | Previously saved login on this computer (Persistent Auth) |
| `portal` or `accountportal` | Account Portal (EOS overlay or browser) |
| `exchangecode` | Exchange code, used when started from the Epic Games Launcher |
| `deviceid` | DeviceId, AUTH_LOGIN is the display name |
| `password` | Email and password (restricted by Epic, not generally available) |
| `devicecode` | Device code |
| `refreshtoken` | Refresh token |
| `external` or `externalauth` | External auth |

## Example Usage
- Logging in to your instance of DevTool at localhost:1111

```
"C:\Program Files\Epic Games\UE_5.6\Engine\Binaries\Win64\UnrealEditor.exe" "C:\EOSProject\EOSProject.uproject" -game -log -AUTH_TYPE=devtool -AUTH_LOGIN=localhost:1111 -AUTH_TOKEN=MrX
```

## Auto-Login with previously saved credentials
```
"C:\Program Files\Epic Games\UE_5.6\Engine\Binaries\Win64\UnrealEditor.exe" "C:\EOSProject\EOSProject.uproject" -game -log -AUTH_TYPE=autologin
```

## Using a different configuration
```
MyGame.exe -EOSConfig=ServerConfig
```
