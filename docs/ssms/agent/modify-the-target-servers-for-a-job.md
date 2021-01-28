---
description: Modify the Target Servers for a Job
title: Modify the Target Servers for a Job
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- modifying target servers
- SQL Server Agent jobs, target servers
- target servers [SQL Server], modifying
ms.assetid: 9dbe24f2-acec-4aa2-920c-e8e96efa18e4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 105eafd450c268eb34bb0ee2be6541b0e6d9ae02
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766330"
---
# <a name="modify-the-target-servers-for-a-job"></a>Modify the Target Servers for a Job

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Artikel wird beschrieben, wie Sie die Zielserver für [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agentaufträge in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] ändern.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Standardmäßig können Mitglieder der festen Serverrolle „sysadmin“ diese gespeicherte Prozedur ausführen. Andere Benutzer müssen Mitglieder einer der folgenden festen SQL Server-Agent-Datenbankrollen in der msdb-Datenbank sein:  
  
1.  **SQLAgentUserRole**  
  
2.  **SQLAgentReaderRole**  
  
3.  SQLAgentOperatorRole  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-modify-the-target-servers-for-a-job"></a>So ändern Sie die Zielserver für einen Auftrag  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, erweitern Sie **Aufträge**, klicken Sie mit der rechten Maustaste auf einen Auftrag, und klicken Sie dann auf **Eigenschaften**.  
  
3.  Wählen Sie im Dialogfeld **Auftragseigenschaften** die Seite **Ziele** aus, und klicken Sie auf **Ziel: Lokaler Server** oder auf **Ziel: Mehrere Server**.  
  
    Wenn Sie **Ziel: Mehrere Server** auswählen, geben Sie Server an, die Ziele für den Auftrag sind, indem Sie das Kästchen links neben dem Servernamen aktivieren. Stellen Sie sicher, dass die Kästchen für die Server, die nicht Ziel für den Auftrag sind, deaktiviert sind.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL  
  
#### <a name="to-modify-the-target-servers-for-a-job"></a>So ändern Sie die Zielserver für einen Auftrag  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde_md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird der Multiserverauftrag „Weekly Sales Backups“ dem Server SEATTLE2 zugewiesen.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_jobserver  
    @job_name = N'Weekly Sales Backups',   
    @server_name = N'SEATTLE2' ;   
GO  
```  
  
Weitere Informationen finden Sie unter [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Automatisierte Verwaltung in einem Unternehmen](../../ssms/agent/automated-administration-across-an-enterprise.md)  
