# Sharing the EOS Platform with Other Plugins

EOSCore creates the EOS platform (`EOS_HPlatform`) that its Online Subsystem uses. Other plugins can use that same platform instead of creating their own, even if they can't depend on the EOSCore modules. EOSCore publishes the platform as a modular feature named **EOSPlatformHandleProvider**.

This page is for C++ plugin authors.

## The Contract
Copy this header into your plugin unchanged. The `#ifndef` guard lets several plugins carry their own copy.

```cpp
#pragma once

#include "CoreMinimal.h"
#include "Features/IModularFeature.h"

#ifndef EOS_PLATFORM_HANDLE_PROVIDER_DEFINED
#define EOS_PLATFORM_HANDLE_PROVIDER_DEFINED

class IEOSPlatformHandleProvider : public IModularFeature
{
public:
	static FName GetModularFeatureName()
	{
		static const FName FeatureName(TEXT("EOSPlatformHandleProvider"));
		return FeatureName;
	}

	/** The EOS_HPlatform, or null while there is none. */
	virtual void* GetEOSPlatformHandle() const = 0;
};

#endif
```

## Getting the Platform

```cpp
#include "Features/IModularFeatures.h"
#include "EOSPlatformHandleProvider.h"

EOS_HPlatform GetSharedEOSPlatform()
{
	IModularFeatures& Features = IModularFeatures::Get();
	const FName Name = IEOSPlatformHandleProvider::GetModularFeatureName();

	if (Features.IsModularFeatureAvailable(Name))
	{
		IEOSPlatformHandleProvider& Provider = Features.GetModularFeature<IEOSPlatformHandleProvider>(Name);
		return static_cast<EOS_HPlatform>(Provider.GetEOSPlatformHandle());
	}

	return nullptr;
}
```

## Lifetime
- EOSCore registers the feature when the Online Subsystem initializes and unregisters it when the Online Subsystem shuts down, **before** it releases the platform.
- Don't keep the handle around. Ask the provider again when you need it, or listen to `IModularFeatures::Get().OnModularFeatureUnregistered()` and stop using the handle when the feature goes away.
- In the editor every PIE instance has its own Online Subsystem and platform, so several providers can be registered at the same time. `GetModularFeature` returns the first one.
- Don't call `EOS_Platform_Release` or `EOS_Shutdown` on the shared platform, EOSCore owns it.
- EOS callbacks run when the platform ticks. EOSCore ticks it, so you don't have to.
