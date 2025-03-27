# C++
EOSCore was primarily made with Blueprints in mind and is not convenient to use in a C++ environment, there is also no benefit to using EOSCore in a C++ environment compared to Blueprints, however it is totally doable.

This example will show you a basic implementation and how to get started but also note that if you decide to use EOSCore in C++ you will be on your own. If you are having issues compiling your C++ project then you will have to solve the issues on your own. One of the reason for this is C++ is extremely compicated and complex and not what the plugin is advertised or even suggested to be used with.

With that out of the way, let’s get started!

## Project.Build.cs
Open up your projects **\Source\Project\Project.Build.cs** file and add **”EOSCore**” as a module dependency. Here is an example of what a Build.cs file can look like that incudes the EOSCore module

## Project.Build.cs Example
```c
using UnrealBuildTool;

public class SteamCPP : ModuleRules
{
   public SteamCPP(ReadOnlyTargetRules Target) : base(Target)
   {
      PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;
   
      PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "SteamCore" });
   }
}
```

## PlayerController
We’re going to setup a basic test function that will attempt to Login using the EOS Connect Interface, here is an example of the Header and Source files:


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
	void OnLoginCallback(const FEOSConnectLoginCallbackInfo& Data);

public:
	FOnLoginCallback m_OnLoginCallback;
};
```

## PlayerController.cpp

```cpp
#include "BaseController.h"

#include "Connect/EOSConnect.h"

void ABaseController::MyTestFunction()
{
	if (UCoreConnect* Connect = GetGameInstance()->GetSubsystem<UCoreConnect>())
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
