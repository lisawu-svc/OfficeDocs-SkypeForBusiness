---
title: 'Lync Server 2013: Viewing reports from Best Practices Analyzer'
ms.reviewer: 
ms.author: kenwith
author: kenwith
TOCTitle: Viewing reports from Best Practices Analyzer
ms:assetid: 7217a47b-36b1-4923-81ea-df754cff29bb
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Gg607690(v=OCS.15)
ms:contentKeyID: 48184465
ms.date: 07/23/2014
mtps_version: v=OCS.15
---

<div data-xmlns="http://www.w3.org/1999/xhtml">

<div class="topic" data-xmlns="http://www.w3.org/1999/xhtml" data-msxsl="urn:schemas-microsoft-com:xslt" data-cs="http://msdn.microsoft.com/en-us/">

<div data-asp="http://msdn2.microsoft.com/asp">

# Viewing reports from Best Practices Analyzer in Lync Server 2013

</div>

<div id="mainSection">

<div id="mainBody">

<span> </span>

_**Topic Last Modified:** 2012-09-21_

When you use Best Practices Analyzer to scan your environment, you specify a name for the scan. After Best Practices Analyzer completes a scan, it stores the scan results in reports and saves them under the name of the scan. Upon completion of the scan, you can view the reports generated for that scan by clicking **View a report of this Best Practices scan** directly from the **Scanning Completed** page. You can also view the reports from that scan or previous scans at a later time. You can view reports on the local computer on which the scan was run, import scan results from another computer, or export scan results to view the reports on another computer on which Best Practices Analyzer is installed.

Scan results are presented in the following types of reports:

  - List reports

  - Tree reports

  - Other reports

These reports include errors, warnings, and other information. For details about each of these types of reports and issues, see [Understanding reports created by Best Practices Analyzer in Lync Server 2013](lync-server-2013-understanding-reports-created-by-best-practices-analyzer.md).

Use the following procedure to view scan results previously generated by Best Practices Analyzer.

<div>

## To view reports from a previous scan

1.  Log on to a computer on which Best Practices Analyzer is installed using an account that is a member of the local User account.
    
    > [!NOTE]  
    > You can view the results of a scan using an account that is a member of the local Administrators group, but you cannot run a scan unless you have appropriate user rights and permissions. For details, see <A href="lync-server-2013-group-memberships-and-user-rights-requirements-for-best-practices-analyzer.md">Group memberships and user rights requirements for Best Practices Analyzer in Lync Server 2013</A>.

2.  Click **Start**, point to **All Programs**, click **Microsoft Lync Server 2013**, and then click **Best Practices Analyzer**.

3.  On the **Welcome** screen, click **Select the scan results to view**.

4.  On the **Select a Best Practices Scan to View** page, do one of the following:
    
      - To view reports from the list of locally stored scan results, click the name of scan, and then click **View a report of this scan**.
        
        > [!NOTE]  
        > The Best Practices Analyzer creates the list of local files from the folder &lt;systemDrive&gt;\\Documents and Settings\\&lt;user&gt;\Application Data\Microsoft\RtcBPA.
    
      - To view reports for results of a scan that are stored at another location, click **Import scan**, locate the file containing the scan results, and then click **Open**.
        
        > [!NOTE]  
        > If the version of Best Practices Analyzer on this computer does not match the version that was used to collect the data in the imported file, the tool on your computer might analyze the file again, after it is imported.

5.  On the **View Best Practices Report** page, do one of the following:
    
      - To view reports in a list organized by server component, click **List Reports**, and then click either the **All Issues** tab or the **Informational Items** tab.
    
      - To view reports as a hierarchical list organized by types of results, click **Tree Reports**, and then click either the **Detailed View** tab or the **Summary View** tab.
    
      - To view other reports, click **Other Reports**.
    
    > [!NOTE]  
    > For details about the Best Practices Analyzer reports and the issues that they identify, see <A href="lync-server-2013-viewing-and-working-with-reports-created-by-best-practices-analyzer.md">Viewing and working with reports created by Best Practices Analyzer in Lync Server 2013</A> and <A href="lync-server-2013-analyzing-and-resolving-issues-identified-by-best-practices-analyzer.md">Analyzing and resolving issues identified by Best Practices Analyzer in Lync Server 2013</A>.

</div>

</div>

</div>

</div>

</div>

