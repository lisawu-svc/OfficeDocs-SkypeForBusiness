---
title: 'Lync Server 2013: Assign a per-user client version policy'
ms.reviewer: 
ms.author: kenwith
author: kenwith
TOCTitle: Assign a per-user client version policy
ms:assetid: f7e8ba2f-62dc-4e7d-8b63-682986f10240
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Gg182607(v=OCS.15)
ms:contentKeyID: 48185868
ms.date: 07/23/2014
mtps_version: v=OCS.15
---

# Assign a per-user client version policy in Lync Server 2013

 


The client version policy is one of the individual settings of a user account that you can configure in the Lync Server Control Panel.

Deploying one or more per-user client version policies is optional. You can also deploy only a global-level client version policy, or site-level or pool-level client version policies. If you do deploy per-user policies, you must explicitly assign them to users, groups, or contact object. When no specific site-level, pool-level, or per-user policy is assigned, the default clients that are allowed to register with Lync Server 2013 are those defined in the global-level client version policy.

After creating at least one per-user client version policy, use the procedures in this topic to assign the policy that specifies the client versions that you want to allow to register with Lync Server.

For details about creating per-user client version policies, see [Specifying the client applications that can be used to log on to Lync Server 2013](lync-server-2013-specifying-the-client-applications-that-can-be-used-to-log-on-to-lync-server-2013.md).

## To assign a per-user client version policy

1.  From a user account that is assigned to the CsUserAdministrator role or the CsAdministrator role, log on to any computer in your internal deployment.

2.  Open a browser window, and then enter the Admin URL to open the Lync Server Control Panel. For details about the different methods you can use to start Lync Server Control Panel, see [Open Lync Server 2013 administrative tools](lync-server-2013-open-lync-server-administrative-tools.md).

3.  In the left navigation bar, click **Users**.

4.  Use one of the following methods to locate a user:
    
      - In the **Search users** box, type all or the first portion of the display name, first name, last name, Security Accounts Manager (SAM) account name, SIP address, or line Uniform Resource Identifier (URI) of the user account, and then click **Find**.
    
      - If you have a saved query, click the **Open query** icon, use the **Open** dialog box to retrieve the query (a .usf file), and then click **Find**.

5.  (Optional) Specify additional search criteria to narrow the results:
    
    1.  Click **Add Filter**.
    
    2.  Enter the user property by typing it or by clicking the arrow in the drop-down list to select the property.
    
    3.  In the **Equal to** drop-down list, click the operator (for example, **Equal to** or **Not equal to**).
    
    4.  Depending on the user property you selected, enter the criteria you want to use to filter the search results by typing it or by clicking the arrow in the drop-down list.
        

        > [!TIP]  
        > To add additional search clauses to your query, click <STRONG>Add Filter</STRONG>.

    
    5.  Click **Find**.

6.  Click a user in the search results, click **Action**, and then click **Assign policies**.
    

    > [!TIP]  
    > If you want the same per-user client version policy to apply to multiple users, select multiple users in the search results, then click <STRONG>Actions</STRONG>, and then click <STRONG>Assign policies</STRONG>.



7.  In **Assign Policies**, under **Client version policy**, do one of the following:
    

    > [!NOTE]  
    > Because there are multiple policies that you can configure by using the <STRONG>Assign Policies</STRONG> dialog box, <STRONG>&lt;Keep as is&gt;</STRONG> is selected by default for every policy in the dialog box. Continue using the policy previously assigned to the user by making no changes to this setting.

    
      - Allow Lync Server to automatically choose either the global-level policy or, if defined, the site-level policy or pool-level policy.
    
      - Click the name of a per-user client version policy you previously defined on the **Client Version Policy** page.
        

        > [!TIP]  
        > To help you decide the policy you want to assign, after you click a policy name, click <STRONG>View</STRONG> to view the user rights and permissions defined in the policy.



8.  When you are finished, click **OK**.

## Assigning a Per-User Client Version Policy by Using Windows PowerShell Cmdlets

You can assign per-user client version policies by using the Grant-CsClientVersionPolicy cmdlet. You can run this cmdlet from the Lync Server 2013 Management Shell or from a remote session of Windows PowerShell. For details about using remote Windows PowerShell to connect to Lync Server, see the Lync Server Windows PowerShell blog article "Quick Start: Managing Microsoft Lync Server 2010 Using Remote PowerShell" at [http://go.microsoft.com/fwlink/p/?linkId=255876](http://go.microsoft.com/fwlink/p/?linkid=255876).

## To assign a per-user client version policy to a single user

  - The following command assigns the per-user client version policy RedmondClientVersionPolicy to the user Ken Myer.
    
        Grant-CsClientVersionPolicy -Identity "Ken Myer" -PolicyName "RedmondClientVersionPolicy"

## To assign a per-user client version policy to multiple users

  - This command assigns the per-user client version policy RedmondClientVersionPolicy to all the users who are currently assigned the voice policy RedmondVoicePolicy. For more information on the Filter parameter used in this command, see the documentation for the [Get-CsUser](https://technet.microsoft.com/en-us/library/gg398125\(v=ocs.15\)) cmdlet.
    
        Get-CsUser -Filter {VoicePolicy -eq "RedmondVoicePolicy"} | Grant-CsClientVersionPolicy -PolicyName "RedmondClientVersionPolicy"

## To unassign a per-user client version policy

  - The following command unassigns any per-user client version policy previously assigned to Ken Myer. After the per-user policy is unassigned, Ken Myer will automatically be managed by using the global policy, his local site policy (if one exists), or the service-scope policy assigned to his Registrar. A service scope policy takes precedence over any site policy, and a site policy takes precedence over the global policy.
    
        Grant-CsClientVersionPolicy -Identity "Ken Myer" -PolicyName $Null

For more information, see the help topic for the [Grant-CsClientVersionPolicy](https://technet.microsoft.com/en-us/library/gg412903\(v=ocs.15\)) cmdlet.

## See Also


[Assigning per-user policies in Lync Server 2013](lync-server-2013-assigning-per-user-policies.md)  
[Managing devices, phones, and client applications in Lync Server 2013](lync-server-2013-managing-devices-phones-and-client-applications.md)

