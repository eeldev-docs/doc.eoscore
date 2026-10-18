---
sidebar_position: 8
---

# Ecom (Epic Games Store)

The Ecom interface handles offers, purchases and entitlements for games sold on the Epic Games Store. It needs an **Epic Account** login (Auth interface), not only a Connect login.

- [EOS Ecom documentation](https://dev.epicgames.com/docs/epic-games-store/services/ecom/ecom-overview)

## Common Nodes

| Node | Use |
|---|---|
| `EOS_Ecom_QueryOffers` | Downloads the offers in your catalog |
| `EOS_Ecom_GetOfferCount` + `EOS_Ecom_CopyOfferByIndex` | Reads offers (title, price, currency) |
| `EOS_Ecom_Checkout` | Opens the purchase flow in the EOS overlay |
| `EOS_Ecom_GetTransactionCount` + `EOS_Ecom_CopyTransactionByIndex` | Reads finished transactions, release them with `EOS_Ecom_Transaction_Release` |
| `EOS_Ecom_QueryEntitlements` | Downloads what the player owns |
| `EOS_Ecom_GetEntitlementsCount` + `EOS_Ecom_CopyEntitlementByIndex` | Reads entitlements |
| `EOS_Ecom_QueryOwnership` | Checks if the player owns specific catalog items, for example DLC |
| `EOS_Ecom_RedeemEntitlements` | Marks consumable entitlements as redeemed |
| `EOS_Ecom_QueryOwnershipToken` / `EOS_Ecom_QueryEntitlementToken` | Signed tokens your backend can verify |

## Notes
- **Checkout** uses the EOS overlay, so it only works in a Standalone or packaged game started with the overlay enabled.
- Grant items from the transaction or from a fresh **QueryEntitlements**, never from the client alone. For important items, verify an ownership or entitlement token on your server.
- The `EOSCore` Online Subsystem Store/Purchase interface is only available when the game was started from the Epic Games Launcher (`-EpicPortal`).
