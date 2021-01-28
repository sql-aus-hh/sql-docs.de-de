---
description: Change the Membership of a Job Category
title: Change the Membership of a Job Category
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, categories
- jobs [SQL Server Agent], categories
- categories [SQL Server Agent jobs]
- members [SQL Server], job categories
ms.assetid: 6a18f7f0-eb50-485f-a9c7-df31ae0f994e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: fc6dd371ae2ef8cccb175b7654d7e1f23dd6405f
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766148"
---
# <a name="change-the-membership-of-a-job-category"></a>Change the Membership of a Job Category
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie Sie die Mitgliedschaft der Auftragskategorie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder SQL Server Management Objects ändern können.  
  
Auftragskategorien helfen Ihnen dabei, Ihre Aufträge zum einfachen Filtern und Gruppieren zu organisieren. Sie können eigene Auftragskategorien erstellen. Zudem können Sie die Mitgliedschaft von Microsoft SQL Server-Agent-Aufträgen in Auftragskategorien ändern.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-change-the-membership-of-a-job-category"></a>So ändern Sie die Mitgliedschaft einer Auftragskategorie  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, auf dem Sie eine Auftragskategorie bearbeiten möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.  
  
3.  Klicken Sie mit der rechten Maustaste auf den Ordner **Aufträge** , und wählen Sie **Auftragskategorien verwalten** aus.  
  
4.  Wählen Sie im Dialogfeld **Auftragskategorien verwalten -**_Servername_ die Auftragskategorie aus, die Sie bearbeiten möchten, und klicken Sie dann auf **Aufträge anzeigen**.  
  
5.  Aktivieren Sie das Kontrollkästchen **Alle Aufträge anzeigen** .  
  
6.  Um der Kategorie einen Auftrag hinzuzufügen, aktivieren Sie im Hauptraster in der **Select** -Spalte das Kontrollkästchen, das dem Auftrag entspricht. Um einen Auftrag aus der Kategorie zu entfernen, deaktivieren Sie das Kontrollkästchen. Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
7.  Schließen Sie das Dialogfeld **Auftragskategorien verwalten -** _Servername_ .  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-change-the-membership-of-a-job-category"></a>So ändern Sie die Mitgliedschaft einer Auftragskategorie  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- adding a new job category to the "NightlyBackups" job  
    USE msdb ;  
    GO  
    EXEC dbo.sp_update_job  
        @job_name = N'NightlyBackups',  
        @category_name = N'[Uncategorized (Local)]';  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Verwendung von SQL Server Management Objects  
**So ändern Sie die Mitgliedschaft einer Auftragskategorie**  
  
Verwenden Sie die **JobCategory** -Klasse, indem Sie eine von Ihnen ausgewählte Programmiersprache, z. B. Visual Basic, Visual C# oder PowerShell verwenden.  
