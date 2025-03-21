---
title: 'Lync Server 2013: Enabling or disabling push notifications for Windows Phones'
ms.reviewer: 
ms.author: kenwith
author: kenwith
TOCTitle: Enabling or disabling push notifications for Windows Phones
ms:assetid: a34f0c5c-4228-40e3-9d93-bc0b5df4895d
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ688162(v=OCS.15)
ms:contentKeyID: 49733767
ms.date: 07/23/2014
mtps_version: v=OCS.15
---

<div data-xmlns="http://www.w3.org/1999/xhtml">

<div class="topic" data-xmlns="http://www.w3.org/1999/xhtml" data-msxsl="urn:schemas-microsoft-com:xslt" data-cs="http://msdn.microsoft.com/en-us/">

<div data-asp="http://msdn2.microsoft.com/asp">

# Enabling or disabling push notifications for Windows Phones in Lync Server 2013

</div>

<div id="mainSection">

<div id="mainBody">

<span> </span>

_**Topic Last Modified:** 2013-02-23_

Push notifications, in the form of badges, icons, or alerts, can be sent to a Windows Phone even when the mobile application is inactive. Push notifications notify a user of events such as a new or missed IM invitation and voice mail. You can enable or disable push notifications for Windows Phone devices by using either Lync Server 2013 Control Panel or Lync Server 2013 Management Shell.

<div>

## To enable push notifications for Windows Phone by using Lync Server Control Panel

1.  From a user account that is assigned to the CsUserAdministrator role or the CsAdministrator role, log on to any computer in your internal deployment.

2.  Open a browser window, and then enter the Admin URL to open the Lync Server Control Panel. For details about the different methods you can use to start Lync Server Control Panel, see [Open Lync Server 2013 administrative tools](lync-server-2013-open-lync-server-administrative-tools.md).

3.  In the left navigation bar, click **Clients**, and then click the **Push Notification Configuration** navigation button.

4.  On the **Push Notification Configuration** page, click the site you want to edit, click the **Edit** menu, and then click **Show details**.

5.  Click the **Enable Microsoft push notifications** checkbox.

6.  Click **Commit**.

</div>

<div>

## To disable push notifications for Windows Phone by using Lync Server Control Panel

1.  From a user account that is assigned to the CsUserAdministrator role or the CsAdministrator role, log on to any computer in your internal deployment.

2.  Open a browser window, and then enter the Admin URL to open the Lync Server Control Panel. For details about the different methods you can use to start Lync Server Control Panel, see [Open Lync Server 2013 administrative tools](lync-server-2013-open-lync-server-administrative-tools.md).

3.  In the left navigation bar, click **Clients**, and then click the **Push Notification Configuration** navigation button.

4.  On the **Push Notification Configuration** page, click the site you want to edit, click the **Edit** menu, and then click **Show details**.

5.  Clear the **Enable Microsoft push notifications** checkbox.

6.  Click **Commit**.

</div>

<div>

## Enabling or Disabling Push Notifications for Windows Phone by Using Windows PowerShell Cmdlets

You can enable or disable push notifications for Windows Phone by using the **Set-CsPushNotificationConfiguration** cmdlet. You can run this cmdlet either from the Lync Server 2013 Management Shell or from a remote session of Windows PowerShell. For details about using remote Windows PowerShell to connect to Lync Server, see the Lync Server Windows PowerShell blog article "Quick Start: Managing Microsoft Lync Server 2010 Using Remote PowerShell" at [http://go.microsoft.com/fwlink/p/?linkId=255876](http://go.microsoft.com/fwlink/p/?linkid=255876).

<div>

## To enable push notifications for Windows Phone

  - To enable push notifications for Windows Phone set the value of the EnableMicrosoftPushNotificationService property to True ($True). For example:
    
        Set-CsPushNotificationConfiguration -Identity "site:Redmond" -EnableMicrosoftPushNotificationService $True

</div>

<div>

## To disable push notifications for Windows Phone

  - To disable push notifications for Windows Phone set the value of the EnableMicrosoftPushNotificationService property to False ($False). For example:
    
        Set-CsPushNotificationConfiguration -Identity "site:Redmond" -EnableMicrosoftPushNotificationService $False

</div>

For more information, see the help topic for the [Set-CsPushNotificationConfiguration](https://docs.microsoft.com/powershell/module/skype/Set-CsPushNotificationConfiguration) cmdlet.

</div>

<div>

## See Also


[Configuring for push notifications in Lync Server 2013](lync-server-2013-configuring-for-push-notifications.md)  
  

</div>

</div>

<span> </span>

</div>

</div>

</div>

