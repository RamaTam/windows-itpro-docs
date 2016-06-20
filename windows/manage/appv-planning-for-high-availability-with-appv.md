---
title: Planning for High Availability with App-V 5.1
description: Planning for High Availability with App-V 5.1
author: jamiejdt
ms.assetid: 1f190a0e-10ee-4fbe-a602-7e807e943033
ms.pagetype: mdop, appcompat, virtualization
ms.mktglfcycl: deploy
ms.sitesec: library
ms.prod: w10
---


# Planning for High Availability with App-V 5.1


Microsoft Application Virtualization (App-V) 5.1 system configurations can take advantage of options that maintain a high level of available service.

Use the information in the following sections to help you understand the options to deploy App-V 5.1 in a highly available configuration.

-   [Support for Microsoft SQL Server clustering](#bkmk-sqlcluster)

-   [Support for IIS Network Load Balancing](#bkmk-iisloadbal)

-   [Support for clustered file servers when running (SCS) mode](#bkmk-clusterscsmode)

-   [Support for Microsoft SQL Server Mirroring](#bkmk-sqlmirroring)

-   [Support for Microsoft SQL Server Always On](#bkmk-sqlalwayson)

## <a href="" id="bkmk-sqlcluster"></a>Support for Microsoft SQL Server clustering


You can run the App-V Management database and Reporting database on computers that are running Microsoft SQL Server clusters. However, you must install the databases using scripts.

For instructions, see [How to Deploy the App-V Databases by Using SQL Scripts](appv-deploy-appv-databases-with-sql-scripts.md).

## <a href="" id="bkmk-iisloadbal"></a>Support for IIS Network Load Balancing


You can use Internet Information Services (IIS) Network Load Balancing to configure a highly available environment for computers running the App-V 5.x Management, Publishing, and Reporting services which are deployed through IIS.

Review the following for more information about configuring IIS and Network Load Balancing for computers running Windows Server operating systems:

-   Provides information about configuring Internet Information Services (IIS) 7.0.

    [Achieving High Availability and Scalability - ARR and NLB](http://go.microsoft.com/fwlink/?LinkId=316369) (http://go.microsoft.com/fwlink/?LinkId=316369)

-   Configuring Microsoft Windows Server

    [Network Load Balancing](http://go.microsoft.com/fwlink/?LinkId=316370) (http://go.microsoft.com/fwlink/?LinkId=316370).

    This information also applies to IIS Network Load Balancing (NLB) clusters in Windows Server 2008, Windows Server 2008 R2, or Windows Server 2012.

    **Note**  
    The IIS Network Load Balancing functionality in Windows Server 2012 is generally the same as in Windows Server 2008 R2. However, some task details are changed in Windows Server 2012. For information on new ways to do tasks, see [Common Management Tasks and Navigation in Windows Server 2012 R2 Preview and Windows Server 2012](http://go.microsoft.com/fwlink/?LinkId=316371) (http://go.microsoft.com/fwlink/?LinkId=316371).

     

## <a href="" id="bkmk-clusterscsmode"></a>Support for clustered file servers when running (SCS) mode


Running App-V 5.1 in Share Content Store (SCS) mode with clustered file servers is supported.

The following steps can be used to enable this configuration:

-   Configure App-V 5.1 to run in client SCS mode. For more information about configuring App-V 5.1 SCS mode, see [How to Install the App-V 5.1 Client for Shared Content Store Mode](appv-install-the-appv-client-for-shared-content-store-mode.md).

-   Configure the file server cluster configured in both the Microsoft Server 2012 scale out mode and pre **2012** mode with a virtual SAN.

The following steps can be used to validate the configuration:

1.  Add a package on the publishing server. For more information about adding a package, see [How to Add or Upgrade Packages by Using the Management Console](appv-add-or-upgrade-packages-with-the-management-console.md).

2.  Perform a publishing refresh on the computer running the App-V 5.1 client and open an application.

3.  Switch cluster nodes mid-publishing refresh and mid-streaming to ensure fail-over works correctly.

Review the following for more information about configuring Windows Server Failover clusters:

-   [Checklist: Create a Clustered File Server](http://go.microsoft.com/fwlink/?LinkId=316372) (http://go.microsoft.com/fwlink/?LinkId=316372).

-   [Use Cluster Shared Volumes in a Windows Server 2012 Failover Cluster](http://go.microsoft.com/fwlink/?LinkId=316373) (http://go.microsoft.com/fwlink/?LinkId=316373).

## <a href="" id="bkmk-sqlmirroring"></a>Support for Microsoft SQL Server Mirroring


Using Microsoft SQL Server mirroring, where the App-V 5.1 management server database is mirrored utilizing two SQL Server instances, for App-V 5.1 management server databases is supported.

Review the following for more information about configuring Microsoft SQL Server Mirroring:

-   [How to: Prepare a Mirror Database for Mirroring (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=316375) (http://go.microsoft.com/fwlink/?LinkId=316375)

-   [Establish a Database Mirroring Session Using Windows Authentication (SQL Server Management Studio)](http://go.microsoft.com/fwlink/?LinkId=316377) (http://go.microsoft.com/fwlink/?LinkId=316377)

The following steps can be used to validate the configuration:

1.  Initiate a Microsoft SQL Server Mirroring session.

2.  Select **Failover** to designate a new master Microsoft SQL Server instance.

3.  Verify that the App-V 5.1 management server continues to function as expected after the failover.

The connection string on the management server can be modified to include **failover partner = &lt;server2&gt;**. This will only help when the primary on the mirror has failed over to the secondary and the computer running the App-V 5.1 client is doing a fresh connection (say after reboot).

Use the following steps to modify the connection string to include **failover partner = &lt;server2&gt;**:

**Important**  
This topic describes how to change the Windows registry by using Registry Editor. If you change the Windows registry incorrectly, you can cause serious problems that might require you to reinstall Windows. You should make a backup copy of the registry files (System.dat and User.dat) before you change the registry. Microsoft cannot guarantee that the problems that might occur when you change the registry can be resolved. Change the registry at your own risk.

 

1.  Login to the management server and open **regedit**.

2.  Navigate to **HKEY\_LOCAL\_MACHINE** \\ **Software** \\ **Microsoft** \\ **AppV** \\ **Server** \\ **ManagementService**.

3.  Modify the **MANAGEMENT\_SQL\_CONNECTION\_STRING** value with the **failover partner = &lt;server2&gt;**.

4.  Restart management service using the IIS console.

    **Note**  
    Database Mirroring is on the list of Deprecated Database Engine Features for Microsoft SQL Server 2012 due to the **AlwaysOn** feature available with Microsoft SQL Server 2012.

     

Click any of the following links for more information:

-   [How to: Prepare a Mirror Database for Mirroring (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=394235) (http://go.microsoft.com/fwlink/?LinkId=394235).

-   [How to: Configure a Database Mirroring Session (SQL Server Management Studio)](http://go.microsoft.com/fwlink/?LinkId=394236) (http://go.microsoft.com/fwlink/?LinkId=394236).

-   [Establish a Database Mirroring Session Using Windows Authentication (SQL Server Management Studio)](http://go.microsoft.com/fwlink/?LinkId=394237) (http://go.microsoft.com/fwlink/?LinkId=394237).

-   [Deprecated Database Engine Features in SQL Server 2012](http://go.microsoft.com/fwlink/?LinkId=394238) (http://go.microsoft.com/fwlink/?LinkId=394238).

## <a href="" id="bkmk-sqlalwayson"></a>Support for Microsoft SQL Server Always On configuration


The App-V 5.1 management server database supports deployments to computers running Microsoft SQL Server with the **Always On** configuration.

## Got a suggestion for App-V?


Add or vote on suggestions [here](http://appv.uservoice.com/forums/280448-microsoft-application-virtualization). For App-V issues, use the [App-V TechNet Forum](https://social.technet.microsoft.com/Forums/home?forum=mdopappv).

## Related topics


[Planning to Deploy App-V](appv-planning-to-deploy-appv.md)

 

 




