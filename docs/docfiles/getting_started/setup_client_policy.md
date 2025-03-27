---
sidebar_position: 4
---

# Setting up Client & Policy Settings

:::tip NOTE
Beware, this client example will grant the client ALL PRIVILEGES, this might not be ideal in a Live project but it will be fine for testing/development purposes.
:::

Navigate to your Product Settings in the Developer Portal and click on the Clients Tab
![Image](../../../static/img/1-1.png)

## Client Policy
Scroll down to the bottom of the page and click on “**ADD NEW CLIENT POLICY**“

### Adding a new Policy
1. Give your Policy a name
1. Select Custom policy type
1. Make sure “User required” is checked
1. Check all the Features as shown below

![Image](../../../static/img/2_policy_settings.png)

### Policy Permissions
- **CLICK ON EVERY FEATURE IN THE LIST (ACHIVEMENTS IN THIS EXAMPLE) AND MAKE SURE “ALL ACTIONS” ARE ALLOWED**

![Image](../../../static/img/3_policy_settings.png)

:::tip
Make sure you do this for all Features
:::

![Image](../../../static/img/4_policy_settings.png)

### Saving Policy Settings
![Image](../../../static/img/2_policy_settings_2.png)

## Client

### Adding a new client
- In the Product Settings > Clients tab, click on “**ADD NEW CLIENT**“

![Image](../../../static/img/5_add_client.png)

- Give your client a Name and select the Policy we just created in the previous step

### Saving the Client

![Image](../../../static/img/6_client_settings.png)

- After you’ve saved your client, confirm that it was added successfully as shown below:

### Confirming Client Creation
![Image](../../../static/img/7_confirm_client.png)

## Unreal Engine Configuration
- Your client is now ready to be used inside Unreal Engine!

![Image](../../../static/img/10_settings.png)

## Epic Account Services

:::tip
This step is optional but required if you want to use Epic Account Authentication.
:::

### Configuring
- Click on “Epic Account Servces“
- Click on “CONFIGURE” to configure your Application 
![Image](../../../static/img/8_configure_eas.png)


### Brand Settings
- Fill in the **“BRAND SETTINGS**” with your own settings that fits your project

![Image](../../../static/img/brand-1.jpg)

### Permissions
- Next click on the “**PERMISSIONS**” tab and allow all permissions (in this example), Basic Profile, Online Presence and Friends

![Image](../../../static/img/permissions.jpg)

### Clients
- Nativate to the “**CLIENTS**” tab and select your EOS client that you just created in the previous step and click on “**SAVE**“

![Image](../../../static/img/9_eas.png)


## Scope Consent
- EOSCore 1.9.3.3 and going forward allows you to customize the scope required when authenticating Epic Accounts, you'll have to make sure that the requested permissions match the client permissions in your developer portal.
- Project Settings configured Scopes are used by the "**[Login](https://eoscore.eeldev.com/docfiles/getting_started/auth/information/)**" node
- Make sure that the settings in Project Settings **match** the permissions in your Developer Portal

![Image](../../../static/img/scope_1.png)

- Example scopes in the developer portal

![Image](../../../static/img/scope_2.png)

- If you are using the [async nodes](https://eoscore.eeldev.com/docfiles/getting_started/auth/information/) to authenticate you can specify the scopes directly in the node.

![Image](../../../static/img/scope_3.png)