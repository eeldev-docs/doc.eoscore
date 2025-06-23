# Commandline Arguments
There are several commandline arguments that the plugin can use

## Available Arguments

- **-AUTO_LOGIN**<br />
Will attempt to Auto-Login if you have saved PersistentAuth credentails saved on your computer (previous successful authentication)
- **-AUTH_TYPE=DEVTOOL**<br />
See supported list below
- **-AUTH_LOGIN=xxx**<br />
Autentication ID for Auto-Login
- **-AUTH_TOKEN=xxx**<br />
Authentication Token for Auto-Login
- **–EOSConfig=configurationName**<br />
Override the default config name used in Project settings
- **-DisableEOSOverlay**<br />
Disables the EOS overlay

## Example Usage
- Logging in to your instance of DevTool at localhost:1111

```
"C:\UE4Editor.exe" "C:\EOSProject\EOSProject.uproject" -game -log -AUTH_TYPE=DEVTOOL -AUTH_LOGIN=localhost:1111 -AUTH_TOKEN=MrX
```

## Auto-Login with previously saved credentials
```
"C:\UE4Editor.exe" "C:\EOSProject\EOSProject.uproject" -game -log -AUTO_LOGIN
```

## Supported AUTH_LOGIN types
- **-AUTH_TYPE=devtool**<br />
Login using the DevTool
- **-AUTH_TYPE=autologin**<br />
Attempt to autologin using saved credentials from previous session
- **-AUTH_TYPE=portal**<br />
Authenticate using the Browser (portal)
- **-AUTH_TYPE=deviceid**<br />
Login using local device id