---
description: Zuweisen eines Auftrags zu einer Auftragskategorie
title: Zuweisen eines Auftrags zu einer Auftragskategorie
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], assigning
- SQL Server Agent jobs, categories
- jobs [SQL Server Agent], categories
- categories [SQL Server Agent jobs]
- SQL Server Agent jobs, assigning
- assigning job to category
ms.assetid: a9ea65a2-1d73-4582-a335-63adeb450cb6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 5c2ab0d3b985a619fbb31492d47aca88e932dbc7
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765514"
---
# <a name="assign-a-job-to-a-job-category"></a>Zuweisen eines Auftrags zu einer Auftragskategorie

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie Sie [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Aufträge zu Auftragskategorien in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] oder SQL Server Management Objects zuweisen können.  
  
Auftragskategorien helfen Ihnen dabei, Ihre Aufträge zum einfachen Filtern und Gruppieren zu organisieren. Sie können z. B. alle Aufträge für die Datenbanksicherung in der Datenbankwartungskategorie organisieren. Sie können Aufträge integrierten Auftragskategorien zuweisen, oder Sie erstellen eine benutzerdefinierte Auftragskategorie und weisen ihr dann Aufträge zu.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-assign-a-job-to-a-job-category"></a>So weisen Sie einen Auftrag einer Auftragskategorie zu  
  
1.  Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, auf dem Sie einer Auftragskategorie einen Auftrag hinzufügen möchten.  
  
2.  Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.  
  
3.  Klicken Sie auf das Pluszeichen, um den Ordner **Aufträge** zu erweitern.  
  
4.  Klicken Sie mit der rechten Maustaste auf den Auftrag, den Sie bearbeiten möchten, und wählen Sie anschließend **Eigenschaften** aus.  
  
5.  Wählen Sie im Dialogfeld **Auftragseigenschaften -** _Auftragsname_ in der Liste **Kategorie** die Auftragskategorie aus, die Sie dem Auftrag zuweisen möchten.  
  
6.  Klicken Sie auf **OK**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-assign-a-job-to-a-job-category"></a>So weisen Sie einen Auftrag einer Auftragskategorie zu  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```sql
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
**So weisen Sie einen Auftrag einer Auftragskategorie zu**  
  
Verwenden Sie die **JobCategory** -Klasse, indem Sie eine von Ihnen ausgewählte Programmiersprache, z. B. Visual Basic, Visual C# oder PowerShell verwenden.  
