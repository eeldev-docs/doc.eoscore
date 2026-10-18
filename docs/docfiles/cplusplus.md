# C++
EOSCore was primarily made with Blueprints in mind and is not convenient to use in a C++ environment, there is also no benefit to using EOSCore in a C++ environment compared to Blueprints, however it is totally doable.

This example will show you a basic implementation and how to get started but also note that if you decide to use EOSCore in C++ you will be on your own. If you are having issues compiling your C++ project then you will have to solve the issues on your own. One of the reason for this is C++ is extremely complicated and complex and not what the plugin is advertised or even suggested to be used with.

With that out of the way, let’s get started!

## Project.Build.cs
Open up your projects **\Source\Project\Project.Build.cs** file and add **EOSCore** as a module dependency. If you also use the Online Subsystem classes directly (for example `FOnlineSubsystemEOSCore`), add **OnlineSubsystemEOSCore** and **OnlineSubsystem** as well. Here is an example of what a Build.cs file can look like that includes the EOSCore module

## Project.Build.cs Example
```csharp
using UnrealBuildTool;

public class MyProject : ModuleRules
{
   public MyProject(ReadOnlyTargetRules Target) : base(Target)
   {
      PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;
   
      PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "EOSCore" });
   }
}
```

## PlayerController
We’re going to setup a basic test function that will attempt to Login using the EOS Connect Interface with a DeviceId. A DeviceId has to be created once per device before it can be used to login, so the example first calls **EOS_Connect_CreateDeviceId** and then logs in. If the DeviceId already exists, CreateDeviceId returns `EOS_DuplicateNotAllowed`, which is fine.

:::note
All EOSCore callbacks are executed on the game thread from the EOS tick. If the plugin is not initialized (no EOS platform), the callback is called right away with `EOS_NotConfigured`.
:::

Here is an example of the Header and Source files:


## PlayerController.h
```cpp
#pragma once

#include "CoreMinimal.h"

#include "Connect/EOSConnectTypes.h"
#include "GameFramework/PlayerController.h"
#include "BaseController.generated.h"

UCLASS()
class ABaseController : public APlayerController
{
	GENERATED_BODY()

public:
	UFUNCTION(BlueprintCallable, Category = "EOS TEST")
	void MyTestFunction();

public:
	UFUNCTION()
	void OnCreateDeviceIdCallback(const FEOSConnectCreateDeviceIdCallbackInfo& Data);

	UFUNCTION()
	void OnLoginCallback(const FEOSConnectLoginCallbackInfo& Data);

private:
	void Login();

public:
	FOnCreateDeviceIdCallback m_OnCreateDeviceIdCallback;
	FOnLoginCallback m_OnLoginCallback;
};
```

## PlayerController.cpp

```cpp
#include "BaseController.h"

#include "Connect/EOSConnect.h"

void ABaseController::MyTestFunction()
{
	if (UCoreConnect* Connect = UCoreConnect::GetConnect(this))
	{
		m_OnCreateDeviceIdCallback.BindUFunction(this, "OnCreateDeviceIdCallback");

		FEOSConnectCreateDeviceIdOptions Options;
		Options.DeviceModel = "PC Windows";

		Connect->EOSConnectCreateDeviceId(this, Options, m_OnCreateDeviceIdCallback);
	}
}

void ABaseController::OnCreateDeviceIdCallback(const FEOSConnectCreateDeviceIdCallbackInfo& Data)
{
	// EOS_DuplicateNotAllowed means a DeviceId already exists on this device
	if (Data.ResultCode == EOSResult::EOS_Success || Data.ResultCode == EOSResult::EOS_DuplicateNotAllowed)
	{
		Login();
	}
}

void ABaseController::Login()
{
	if (UCoreConnect* Connect = UCoreConnect::GetConnect(this))
	{
		m_OnLoginCallback.BindUFunction(this, "OnLoginCallback");
		
		FEOSConnectLoginOptions Options;
		Options.UserLoginInfo.DisplayName = "Test";
		Options.Credentials.Type = EEOSEExternalCredentialType::EOS_ECT_DEVICEID_ACCESS_TOKEN;

		Connect->EOSConnectLogin(this, Options, m_OnLoginCallback);
	}
}

void ABaseController::OnLoginCallback(const FEOSConnectLoginCallbackInfo& Data)
{
	if (Data.ResultCode == EOSResult::EOS_Success)
	{
		// Login successful
	}
	else if (Data.ResultCode == EOSResult::EOS_InvalidUser)
	{
		// First login with this DeviceId: create the EOS user with EOS_Connect_CreateUser and Data.ContinuanceToken
	}
	else
	{
		// Login failed
	}
}
```

Now it’s simply a matter of calling the test function from Blueprints

## Example Project
You can download this sample project from Github:

- [https://github.com/eeldev-ue4/EOSCPP_Example](https://github.com/eeldev-ue4/EOSCPP_Example)
