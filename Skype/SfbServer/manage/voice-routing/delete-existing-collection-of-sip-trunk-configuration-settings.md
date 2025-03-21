---
title: "Delete an existing collection of SIP trunk configuration settings in Skype for Business Server"
ms.reviewer: 
ms.author: jambirk
author: jambirk
manager: serdars
ms.audience: ITPro
ms.topic: article
ms.prod: skype-for-business-itpro
localization_priority: Normal
description: "SIP trunk configuration settings define the relationship and capabilities between a Mediation Server and the public switched telephone network (PSTN) gateway, an IP-public branch exchange (PBX), or a Session Border Controller (SBC) at the service provider. "
---

# Delete an existing collection of SIP trunk configuration settings in Skype for Business Server

SIP trunk configuration settings define the relationship and capabilities between a Mediation Server and the public switched telephone network (PSTN) gateway, an IP-public branch exchange (PBX), or a Session Border Controller (SBC) at the service provider. These settings do such things as specify:

- Whether media bypass should be enabled on the trunks.
- The conditions under which real-time transport control protocol (RTCP) packets are sent.
- Whether or not secure real-time protocol (SRTP) encryption is required on each trunk.

When you install Skype for Business Server, a global collection of SIP trunk configuration settings is created for you. This global collection of settings cannot be deleted. However, you can use the Skype for Business ServerControl Panel or the [Remove-CsTrunkConfiguration](https://docs.microsoft.com/en-us/powershell/module/skype/Remove-CsTrunkConfiguration) cmdlet to "reset" the properties in the global collection to their default values. For example, if you have set the Enable3pccRefer property to True, when you reset the global collection the Enable3pccRefer property will revert to its default value of False.

Administrators can also create custom trunk configuration settings at the site scope or at the service scope (for an individual PSTN gateway); these custom settings can be removed. When removing these custom settings keep the following in mind:

- If you remove service scope settings, then the SIP trunk managed by those settings will be managed by the settings applied to their site, if they exist. If site settings do not exist, those trunks will then be managed by the global collection of trunk configuration settings.
- If you remove site-scoped settings then any SIP trunks managed by those settings will now be managed by the global collection of trunk configuration settings.

**To remove trunk configuration settings with the Skype for Business Server Control Panel** 

1. In the Skype for Business Server Control Panel, click **Voice Routing**, and then click **Trunk Configuration**.
2. On the **Trunk Configuration** tab, select the collection of SIP trunk configuration settings to be deleted, click **Edit**, and then click **Delete**. To delete multiple collections in the same operation, click the first collection to be deleted, then hold down the Ctrl key and click any additional collections that you want to remove.
3. The **State** property for the collection will be updated to **Uncommitted**. To commit the changes, and to delete the collection, click **Commit** and then click **Commit All**.
4. In the **Uncommitted Voice Configuration Settings** dialog box, click **OK**.
5. In the **Skype for Business Server Control Panel** dialog box, click **OK**.
6. If you change your mind and decide not to delete the collection, click **Commit**, and then click **Cancel All Uncommitted Changes**. When the **Skype for Business Server Control Panel** dialog box appears, click **OK**.

## Removing trunk configuration settings by using Windows PowerShell cmdlets


You can delete trunk configuration settings by using Windows PowerShell and the **Remove-CsTrunkConfiguration** cmdlet. You can run this cmdlet either from the Skype for Business Server Management Shell or from a remote session of Windows PowerShell. 

**To remove a specified collection of settings**

The following command removes the trunk configuration settings applied to the Redmond site:

`Remove-CsTrunkConfiguration -Identity site:Redmond`

**To remove all the collections applied to the site scope**

This command removes all the trunk configuration settings applied to the service scope:

`Get-CsTrunkConfiguration -Filter "service:*" | Remove-CsTrunkConfiguration`

**To remove all the collections where media bypass is enabled**

The following command removes all the trunk configuration settings where media bypass has been enabled:

`Get-CsTrunkConfiguration | Where-Object {$_.EnableBypass -eq $True} | Remove-CsTrunkConfiguration`

For more information, see the help topic for the [Remove-CsTrunkConfiguration](https://docs.microsoft.com/en-us/powershell/module/skype/Remove-CsTrunkConfiguration) cmdlet.
