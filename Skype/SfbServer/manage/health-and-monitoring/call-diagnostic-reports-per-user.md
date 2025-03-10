---
title: "Call Diagnostic Reports (per user) in Skype for Business Server"
ms.reviewer: 
ms.author: jambirk
author: jambirk
manager: serdars
ms.audience: ITPro
ms.topic: article
ms.prod: skype-for-business-itpro
localization_priority: Normal
ms.assetid: 9da13470-001e-415f-b8c5-29b1f3b531ba
description: "Summary: Learn about the per-user Call Diagnostic Reports used in Skype for Business Server."
---

# Call Diagnostic Reports (per user) in Skype for Business Server
  
The Call Diagnostic Reports provide per-user information about failed peer-to-peer and conferencing sessions. At this time there is only one report, the **User Activity Report**.

The User Activity Report provides a detailed list of the peer-to-peer and conferencing sessions carried out by your users in a given time period. Unlike many of the Monitoring Reports, the User Activity Report ties each call to individual users. For example, peer-to-peer sessions specify the SIP URIs of the person who initiated the call (the From user) and the person who was being called (the To user). If you expand the information for a conference, you'll see a list of all the conference participants and the role they held for that conference.

The User Activity Report is sometimes referred to as the "help desk" report. That's because the report is often used by help desk personnel to retrieve session information for a specific user. You can filter for calls made to or made by an individual user simply by typing the user's SIP URI in the User URI prefix box.

If you do this, the User Activity Report will return information for any user whose SIP URI begins with the specified string. For example, if you type **ken** in the URI box, the User Activity Report will locate **Ken**.Myer@litwareinc.com. However, it will also locate these users:

- **ken** azi@litwareinc.com

- **ken** burg@litwareinc.com

- **Ken**.Sanchez@litwareinc.com

- **Ken** nedy@litwareinc.com

To ensure that information only for Ken Myer is returned, either type his full URI (Ken.Myer@litwareinc.com) in the search box or at least enough type of Ken's URI to uniquely distinguish him from other users in your organization. For example:

Ken.my

## To access the user activity report

The User Activity Report is accessed from the Monitoring Reports home page. You can also reach the User Activity Report by clicking the User URI metric on the [IP Phone Inventory Report in Skype for Business Server](ip-phone-inventory-report.md). From within the User Activity Report, clicking the Conference URI (for a conference) takes you to the Conference Detail Report. Similarly, clicking the Detail metric for a peer-to-peer call takes you to the [Peer-to-Peer Session Detail Report in Skype for Business Server](peer-to-peer-session-detail-report.md).

## Making the best use of the user activity report

Although there is a lot of good information in the User Activity Report, that information can sometimes be difficult to locate. For example, all the user activity that takes place in your organization during a specified period is included in the User Activity Report; that means that, buried, within the report is information about which users actually used Skype for Business Server in some way.

> [!NOTE]
> Technically, it's possible that some user activity might go unrecorded: while Skype for Business Server strives to keep information about all phone calls it's possible that a call could have been made without the information about that call being written to the database. Skype for Business Server is designed to give an extremely accurate but not necessarily perfect look at how Skype for Business Server is being used. (The fact that there is no guarantee that 100% of all calls are recorded explains why Skype for Business Server monitoring should not be used as a billing system.) Second, a Monitoring Report can only display, at most, 1,000 records. Depending on the amount of user activity you have, and depending on the time period you are working with, that means your query might not return all the data actually stored in the database. 

- Which users actually used the system during this time period?

- Which of my users were the most active during this time period?

- Are the users who make the most phone calls also the users who participate in the most instant messaging sessions?

If you need to answer questions like this, you can export the data retrieved by the Monitoring Reports to an Excel spreadsheet. You then use that spreadsheet and/or a comma-separated values file to analyze the data in ways that the User Activity Report. For example, suppose you have exported the report data to Excel and then to a comma-separated values file. At that point, you can import the data from the .CSV file to Windows PowerShell by using a command similar to this:

```
$x = Import-Csv -Path "C:\Data\User_Activity_Report.csv"
```

After the data has been imported you can then use simple Windows PowerShell commands to help answer your questions. For example, this command returns a list of unique users who served as the "From user" in at least one session:

```
$x | Group-Object "From user" | Select Name | Sort-Object Name
```

In other words:

<pre>
Name
----
David.Ahs@litwareinc.com
Gilead.Amosnino@litwareinc.com
Henrik.Jensen@litwareinc.com
Ken.Myer@litwareinc.com
Pilar.Ackerman@litwareinc.com
</pre>

This command lists the unique users (based on the total number of sessions that they participated in:

```
$x | Group-Object "From user" | Select Count, Name | Sort-Object Count -Descending
```

That returns data similar to this:

<pre>
Count    Name
-----    ----
  523    Ken.Myer@litwareinc.com
   63    David.Ahs@litwareinc.com
   29    Pilar.Ackerman@litwareinc.com
   17    Gilead.Amosnino@litwareinc.com
   10    Henrik.Jensen@litwareinc.com
</pre>

This command limits the reported sessions to those that included audio as a modality:

```
$x | Where-Object {$_.Modalities -match "audio"} | Group-Object "From user" | Select Count, Name | Sort-Object Count -Descending
```

If you hold your mouse over any Diagnostic ID shown on the report, a tooltip will appear describing that ID.

## Filters

Filters provide a way for you to return a more finely-targeted set of data or to view the returned data in different ways. For example, the User Activity Report enables you to filter the returned data based on such things as activity type (that is, peer-to-peer sessions or conferencing sessions) or by the user's SIP address (allowing you to view the activities for one user). You can also choose how data should be grouped. In this case, usages are grouped by hour, day, week, or month.

The following table lists the filters that you can use with the User Activity Report.

**User activity report filters**


| **Name**   | **Description**  |
|:-----------|:--------|
| **From** <br/>             | Start date/time for the time range. To view data by hours, enter both the start date and time as follows:  <br/> 7/17/12015 1:00 PM  <br/> If you do not enter a start time, the report automatically begins at 12:00 AM on the specified day. To view data by day, enter just the date:  <br/> 7/17/12015  <br/> To view by week or by month, enter a date that falls anywhere within the week or month that you want to view (you do not have to enter the first day of the week or month):  <br/> 7/13/2015  <br/> Weeks always run from Sunday through Saturday.  <br/>                                                      |
| **To** <br/>               | End date/time for the time range. To view data by hours, enter both the end date and time as follows:  <br/> 7/17/12015 1:00 PM  <br/> If you do not enter an end time, the report automatically ends at 12:00 AM on the specified day. To view data by day, enter just the date:  <br/> 7/17/12015  <br/> To view by week or by month, enter a date that falls anywhere within the week or month that you want to view (you do not have to enter the first day of the week or month):  <br/> 7/13/2015  <br/> Weeks always run from Sunday through Saturday.  <br/>                                                             |
| **Activity type** <br/>    | Type of activity. Select one of the following: <br/>  [All] <br/>  Peer-to-peer <br/>  Conference <br/>      |
| **Modality** <br/>         | The Modality available to you varies depending on the select Activity Type. If the Activity Type is Peer-to-Peer, you can select IM; File Transfer; Application Sharing; Voice; or Video as the modality.  <br/> If the Activity Type is Conference, you can select IM Phone conference; Web conference; Application Sharing; Voice/Video conference; or Telephony conference.  <br/>         |
| **Session category** <br/> | Indicates whether the activity in question succeeded or failed. Select one of the following: <br/>  [All] <br/>  Success <br/>  Expected failure <br/>  Unexpected failure <br/>  An "expected failure" is a failure that is expected to happen; for example, if a user has set his or her status to Do Not Disturb you would expect any call to that user to fail. An "unexpected failure" is a failure that occurs in what would appear to be an otherwise healthy system. For example, a call should not be terminated if the caller is placed on hold. If that occurs, that would be flagged as an unexpected failure. <br/> |
| **User URI prefix** <br/>  | SIP address for the user. To view records only for the user Ken Myer you need to enter Ken Myer's SIP address. For example:  <br/> sip:kenmyer@litwareinc.com  <br/>

## Metrics for peer-to-peer sessions

The following table lists the information provided in the User Activity Report for peer-to-peer sessions (that is, sessions involving just two participants).

**Metrics for peer-to-peer sessions**

|**Name**|**Can you sort on this item?**|**Description**|
|:-----|:-----|:-----|
|**Detail** <br/> |No  <br/> |When you click this item, the report shows you the Peer-to-Peer Session Detail Report for the selected session.  <br/> |
|**From user** <br/> |Yes  <br/> |SIP address of the user who initiated the peer-to-peer session.  <br/> |
|**To user** <br/> |Yes  <br/> |SIP address of the user who joined the peer-to-peer session.  <br/> |
|**Modalities** <br/> |Yes  <br/> |Type of communication used in the session. For example, IM, audio, or file transfer.  <br/> |
|**Invite time** <br/> |Yes  <br/> |Date and time the initial invitation to join the peer-to-peer session was sent.  <br/> |
|**Response time** <br/> |Yes  <br/> |Date and time that the "To" user accepted the session invitation.  <br/> |
|**End time** <br/> |Yes  <br/> |Date and time the peer-to-peer session ended.  <br/> |
|**Diagnostic ID** <br/> |Yes  <br/> |Unique identifier (in the form of an ms-diagnostics header) attached to a SIP message that often provides information useful in troubleshooting errors. Diagnostics headers are optional (it is possible to have SIP sessions that do not include these headers), and diagnostic IDs are reported only for sessions that experienced problems of some kind.  <br/> |

## Metrics for conferencing sessions

The following table lists the information provided in the User Activity Report for conferencing sessions (that is, sessions involving three or more participants).

**Metrics for conferencing sessions**

|**Name**|**Can you sort on this item?**|**Description**|
|:-----|:-----|:-----|
|**Conference URI** <br/> |Yes  <br/> |Unique conference identifier. When you click this item, the report shows you the Conference Detail Report for the selected session. When you expand this item, the report shows you information about the conference participants. For details, see the "Metrics for Conference Participants" section later in this topic.  <br/> |
|**Organizer** <br/> |Yes  <br/> |SIP address of the user who organized the conference.  <br/> |
|**Pool** <br/> |Yes  <br/> |Name of the Edge Server (if any) used in the conference.  <br/> |
|**Start time** <br/> |Yes  <br/> |Date and time that the conference began.  <br/> |
|**End time** <br/> |Yes  <br/> |Date and time that the conference ended.  <br/> |

## Metrics for conference participants

The following table lists the information provided in the User Activity Report provides for each participant in a conference.

**Metrics for conference participants**

|**Name**|**Can you sort on this item?**|**Description**|
|:-----|:-----|:-----|
|**Role** <br/> |No  <br/> |Conference role (for example, Presenter) for the user.  <br/> |
|**Participant** <br/> |No  <br/> |SIP address of the user.  <br/> |
|**Connectivity** <br/> |No  <br/> |Network connection type. For example "From Internal" for internal connection or "From PSTN" for dial-in users.  <br/> |
|**Join time** <br/> |No  <br/> |Date and time that the user joined the conference.  <br/> |
|**Leave time** <br/> |No  <br/> |Date and time that the user left the conference.  <br/> |
|**Diagnostic ID** <br/> |No  <br/> |Unique identifier (in the form of an ms-diagnostics header) attached to a SIP message that often provides information useful in troubleshooting errors. Diagnostics headers are optional (it is possible to have SIP sessions that do not include these headers), and diagnostic IDs are reported only for sessions that experienced problems of some kind.  <br/> |

