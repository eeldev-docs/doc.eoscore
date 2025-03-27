---
sidebar_position: 3
---

# Android (UE 4.25)
Recent updates of the EOS SDK brought some changes that are not compatible with **UE 4.25** by default, so we need to make some additional changes to make EOS work,

### EOS-SDK.AAR
- Download EOS-SDK.AAR from this location:
- [https://eeldev.com/ue/eoscore/4.25/eos-sdk.aar](https://eeldev.com/ue/eoscore/4.25/eos-sdk.aar)

This is a modified version that will work for UE 4.25, you need to replace the **EOS-SDK.AAR** file with the one that is installed from the Epic Launcher, the location of this file is (by default):

```
C:\Program Files\Epic Games\UE_4.25\Engine\Plugins\Marketplace\EOSCore\Source\ThirdParty\EOSLibrary\Bin
```
After replacing the **EOS-SDK.AAR** file with the one you downloaded from the location above everything should work as expected.