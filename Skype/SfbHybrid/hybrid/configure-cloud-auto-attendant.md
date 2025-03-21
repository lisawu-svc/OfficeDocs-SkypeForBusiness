---
title: "Configure Cloud Auto Attendants"
ms.author: jambirk
author: jambirk
manager: serdars
ms.reviewer: wasseemh
ms.audience: ITPro
ms.topic: article
ms.prod: skype-for-business-itpro
localization_priority: Normal
ms.collection: 
description: "Instructions for implementing a Cloud Auto Attendant." 
---

# Configure Cloud Auto Attendants

Skype for Business Server 2019 hybrid implementations only use Cloud Voicemail and Cloud Auto Attendants and do not integrate with Exchange Online.

In Skype for Business Server 2019 you are now able to use the cloud auto attendant feature described in [What are Phone System Auto Attendants](/MicrosoftTeams/what-are-phone-system-auto-attendants.md).

To use Cloud Auto Attendant with Skype for Business Server 2019, you will need to create on-premises resource accounts that act as application endpoints and can be assigned phone numbers, then use the online Admin Center to configure the overall Cloud AA experience. Typically you will have multiple Auto Attendants, each of which plays an audio outgoing message to callers, each of which is mapped to one of these on-premises resource accounts.

If you have an existing Auto Attendant implemented in Exchange UM, before you switch to Exchange Server 2019 or Exchange online, you will need to manually record the details as described below and then implement a completely new Cloud Auto Attendant system using the Office Online Admin portal.

## Server configuration steps

These steps are necessary whether you are creating a brand new Auto Attendant or rebuilding an Auto Attendant originally created in Exchange UM.

Log in to the front end server and run the following PowerShell cmdlets:

1. Create each Cloud Auto Attendant's on-premises [resource account](/MicrosoftTeams/manage-resource-accounts.md) by running the `New-CsHybridApplicationEndpoint` cmdlet as needed (for one or more Auto Attendants), and give each one a name, sip address, and so on. Each auto attendant node requires a resource account.

    For the Main Auto Attendant that will have the Initial Greeting or after-hours greeting, be sure to assign the phone number using the  -LineURI option. This is optional if the auto attendant is a child in the hierarchy. Remember the hierarchy structure will be configured online, on the server we're just creating containers to arrange later.

    ``` Powershell
    New-CsHybridApplicationEndpoint -DisplayName AANode1 -SipAddress sip:aanode1@litwareinc.com -OU "ou=Redmond,dc=litwareinc,dc=com" -LineURI tel:+14255550100
    ```

    or for a child auto attendant you can omit the phone number as shown:

    ``` Powershell
    New-CsHybridApplicationEndpoint -DisplayName AANode1 -SipAddress sip:aanode1@litwareinc.com -OU "ou=Redmond,dc=litwareinc,dc=com"
    ```

    See [New-CsHybridApplicationEndpoint](https://docs.microsoft.com/en-us/powershell/module/skype/new-cshybridapplicationendpoint?view=skype-ps) for more details on this command.

    > [!NOTE]
    > You can also use the `Set-CsHybridApplicationEndpoint` command to a assign a phone number (with the -LineURI option) to the Auto Attendant node that will serve as the initial greeting. This is optional if the auto attendant is a child in the hierarchy. You could do this step later if desired, or reset the phone number at a later time.

    ``` Powershell
    Set-CsHybridApplicationEndpoint -Identity "CN={4f6c99fe-7999-4088-ac4d-e88e0b3d3820},OU=Redmond,DC=litwareinc,DC=com" -DisplayName AANode1 -LineURI tel:+14255550100
    ```

    See [Set-CsHybridApplicationEndpoint](https://docs.microsoft.com/en-us/powershell/module/skype/set-cshybridapplicationendpoint?view=skype-ps) for more details on this command.

2. (Optional) Once these resource accounts are created and the required phone numbers are assigned, you can either wait for AD to sync between online and on premise, or force a sync and proceed to online configuration of the Auto Attendants. To force a sync you would run the following command on the computer running AAD Connect (if you haven't done so already you would need to load `import-module adsync` to run the command):

    ``` Powershell
    Start-ADSyncSyncCycle -PolicyType Delta
    ```

    See [Start-ADSyncSyncCycle](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-feature-scheduler) for more details on this command.

## Online configuration steps

Your online implementation will need to have a plan that includes Phone System licenses as described at [Office 365 Enterprise E1, E3, and E4](https://docs.microsoft.com/en-us/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/license-options-based-on-your-plan/office-365-enterprise-e1-e3-e4) or [Office 365 Enterprise E5](https://docs.microsoft.com/en-us/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/license-options-based-on-your-plan/office-365-enterprise-e5-with-audio-conferencing).

1. Assign Phone System licenses to the on-premise hybrid endpoints (auto attendant placeholders) as described in  [Assign Skype for Business and Microsoft Teams licenses](https://docs.microsoft.com/en-us/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/assign-skype-for-business-and-microsoft-teams-licenses).

2. Use the procedures in [Set up a Phone System auto attendant](/MicrosoftTeams/set-up-a-phone-system-auto-attendant.md) to implement the Cloud Auto Attendant structure, including redirects to users, to nested auto attendants, to call queues, or other valid options.  

An example of a small business implementation is available in [Small business example - Set up an auto attendant](/SkypeForBusiness/what-is-phone-system-in-office-365/tutorial-org-aa.yml).

## Test the new Auto Attendant

The best way to test the implementation is to call the number configured for an Auto Attendant and choose options to navigate to each of the auto attendants you've just created. You can also quickly place a test call to your Auto Attendant by using the **Test button** in the Microsoft Teams admin center Action pane. If you want to make changes to an Auto Attendant, select the Auto Attendant, and then in the Action pane click **Edit**.

## Manually moving an Exchange UM Auto Attendant to Cloud Auto Attendant

1. Get a list of all Auto Attendants by running the following command on the Exchange 2013 or 2016 system while logged in as admin:

    ``` Powershell
    Get-UMAutoAttendant | Format-List
    ```

2. For each listed Auto Attendant, note its place in the structure, settings, and get copies of associated sound or text-to-speech file (the guid in the output will be the name of a folder where the files are stored). You can get these details by running the command:

    ``` Powershell
    Get-UMAutoAttendant -Identity MyUMAutoAttendant
    ```

    See [Get-UMAutoAttendant](https://docs.microsoft.com/en-us/powershell/module/exchange/unified-messaging/get-umautoattendant?view=exchange-ps) for more details on this command. A complete list of Auto Attendant options you might need to capture is at [UMAutoAttendant members](https://msdn.microsoft.com/en-us/library/microsoft.exchange.data.directory.systemconfiguration.umautoattendant_members.aspx) but the most important options to note down are:
    - Business hours
    - Non-business hours
    - Language
    - Holiday schedule

3. Create new on-premises endpoints as described above in [Server configuration steps](#server-configuration-steps).
  Assign the main auto Attendant a temporary number for testing purposes

4. Configure a Cloud Auto Attendant system that uses these endpoints as described above in [Online configuration steps](#online-configuration-steps).  
  You may find it useful to use the exercises in the tutorial titled [Small business example - Set up an auto attendant](/SkypeForBusiness/what-is-phone-system-in-office-365/tutorial-org-aa.yml) to create a logical map of the Auto Attendant and user hierarchies in your old Exchange UM Auto Attendant.
5. Test the Cloud Auto Attendant system.
6. Reassign the phone number linked to the Exchange UM Auto Attendant to the Cloud Auto Attendant.

At this point, if you have already migrated UM Voicemail, you should be in a position to migrate to Exchange Server 2019.

## See Also

[What are Phone System auto attendants?](/MicrosoftTeams/what-are-phone-system-auto-attendants.md)

[Set up a Phone System auto attendant](/SkypeForBusiness/what-is-phone-system-in-office-365/set-up-a-phone-system-auto-attendant.md)

[Plan Cloud Auto Attendant](plan-cloud-auto-attendant.md)

[Plan Cloud Voicemail service](plan-cloud-voicemail.md)

[Configure Cloud Voicemail service](configure-cloud-voicemail.md)

[New-CsHybridApplicationEndpoint](https://docs.microsoft.com/powershell/module/skype/new-cshybridapplicationendpoint?view=skype-ps)

[New-CsOnlineApplicationInstance](https://docs.microsoft.com/powershell/module/skype/new-csonlineapplicationinstance?view=skype-ps)
