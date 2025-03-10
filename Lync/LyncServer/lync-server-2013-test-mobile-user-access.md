---
title: 'Lync Server 2013: Test mobile user access'
ms.reviewer: 
ms.author: kenwith
author: kenwith
TOCTitle: Test mobile user access
ms:assetid: 81d97420-428b-41b7-91ef-185d572d3456
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Dn767947(v=OCS.15)
ms:contentKeyID: 63969624
ms.date: 01/27/2015
mtps_version: v=OCS.15
---

<div data-xmlns="http://www.w3.org/1999/xhtml">

<div class="topic" data-xmlns="http://www.w3.org/1999/xhtml" data-msxsl="urn:schemas-microsoft-com:xslt" data-cs="http://msdn.microsoft.com/en-us/">

<div data-asp="http://msdn2.microsoft.com/asp">

# Test mobile user access in Lync Server 2013

</div>

<div id="mainSection">

<div id="mainBody">

<span> </span>

_**Topic Last Modified:** 2014-06-07_


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Verification schedule</p></td>
<td><p>Monthly</p></td>
</tr>
<tr class="even">
<td><p>Testing tool</p></td>
<td><p>Windows PowerShell</p></td>
</tr>
<tr class="odd">
<td><p>Permissions required</p></td>
<td><p>When run locally using the Lync Server Management Shell, users must be members of the RTCUniversalServerAdmins security group.</p>
<p>When run using a remote instance of Windows PowerShell, users must be assigned an RBAC role that has permission to run the Test-CsMcxConference cmdlet. To see a list of all RBAC roles that can use this cmdlet, run the following command from the Windows PowerShell prompt:</p>
<pre><code>Get-CsAdminRole | Where-Object {$_.Cmdlets -match &quot;Test-CsMcxConference&quot;}</code></pre></td>
</tr>
</tbody>
</table>


<div>

## Description

The Mobility Service enables mobile device users to do such things as:

1.  Exchange instant messages and presence information.

2.  Store and retrieve voice mail internally instead of with their wireless provider.

3.  Take advantage of Lync Server capabilities such as Call via Work and dial-out conferencing. The Test-CsMcxConference cmdlet provides a quick and easy way to verify that users can join and participate in Lync Server conferences by using a mobile device.

</div>

<div>

## Running the test

To run this check, you must create three Windows PowerShell credentials objects (objects that contain the account name and password) for each account. You must then include those credentials objects and the SIP addresses of the two accounts when you call Test-CsMcxConference as shown in the following example:

    $organizerCred = Get-Credential "litwareinc\kenmyer"
    $user1Cred = Get-Credential "litwareinc\packerman"
    $user2Cred = Get-Credential "litwareinc\adelaney"
    
    Test-CsMcxConference -TargetFqdn "atl-cs-001.litwareinc.com" -Authentication Negotiate -OrganizerSipAddress "sip:kenmyer@litwareinc.com" -OrganizerCredential $organizerCred -UserSipAddress "sip:pilar@litwareinc.com" -UserCredential $user1Cred -User2SipAddress "sip:adelaney@litwareinc.com" -User2Credential $user2Cred

For more information, see the help topic for the [Test-CsMcxConference](https://docs.microsoft.com/powershell/module/skype/Test-CsMcxConference) cmdlet.

</div>

<div>

## Determining success or failure

If the check succeeds, Test-CsMcxConference will report a test result of Success:

Target Fqdn : atl-cs-001.litwareinc.com

Target Uri : http://atl-cs-001.litwareinc.com:443/mcx

Result : Success

Latency : 00:00:00

Error Message :

Diagnosis :

If the check is unsuccessful Test-CsMcxConference will report a test result of Failure. This test result will typically be accompanied by a detailed error message and diagnosis. For example:

Target Fqdn : atl-cs-001.litwareinc.com

Target Uri : https://atl-cs-001.litwareinc.com:443/mcx

Result : Failure

Latency : 00:00:00

Error Message : No response received for Web-Ticket service.

Inner Exception:The HHTP request is unauthorized with client

negotiation scheme 'Ntlm'. The authentication header received

from the server was 'Negotiate'.

Inner Exception:The remote server returned an error: (401)

Unauthorized.

Diagnosis :

Inner Diagnosis:X-MS-server-Fqdb : atl-cs-001.litwareinc.com

Cache-Control : private

Content-Type : text/html; charset=utf-8.

Server : Microsoft-IIS/8.5

WWW-Authenticate : Negotiate,NTLM

X-Powered-By : ASP.NET

X-Content-Type-Options : nosniff

Date : Wed, 28 May 2014 19:22:19 GMT

Content-Length : 6305

</div>

<div>

## Reasons why the test might have failed

If Test-CsMcxConference fails you should begin by verifying that the mobility service is running and can be accessed. That can be done by using a web browser to verify that the mobility service URL for your Lync Server pool can be accessed. For example, this command verifies the URL for the pool atl-cs-001.litwareinc.com:

`https://atl-cs-001.litwareinc.com/mcx/mcxservice.svc`

If the mobility service can be accessed you should then verify that your test users have valid Lync Server accounts. You can retrieve account information by using a command similar to this:

    Get-CsUser -Identity "sip:kenmyer@litwareinc.com" | Select-Object Enabled

If the Enabled property is not equal to True or if the command fails, that means that the user does not have a valid Lync Server account.You should also verify that each user account is enabled for mobility. To do that, first determine the mobility policy that is assigned to the account:

    Get-CsUser -Identity "sip:kenmyer@litwareinc.com" | Select-Object MobilityPolicy

After you know the policy name, use the Get-CsMobilityPolicy cmdlet to verify that the policy in question (for example, RedmondMobilityPolicy) has the EnableMobility property set to True:

    Get-CsMobilityPolicy -Identity "RedmondMobilityPolicy"

If you receive an “authentication header” error message when you run Test-CsMcxConference that often means that you have not specified a valid user account, Verify the user name and password and then try the test again. If you are convinced that the user account is valid, then use the Get-CsWebServiceConfiguration cmdlet and check the value of the UseWindowsAuth property. That will tell you which authentication methods are enabled in your organization.

For more tips about how to troubleshoot the mobility service, see the blog post [Troubleshooting External Lync Mobility Connectivity Issues Step-by-Step](http://blogs.technet.com/b/nexthop/archive/2012/02/21/troubleshooting-external-lync-mobility-connectivity-issues-step-by-step.aspx).

</div>

</div>

<span> </span>

</div>

</div>

</div>

