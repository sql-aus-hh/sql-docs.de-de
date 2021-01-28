---
description: Change the Scheduling Details for a SQL Server Agent Master Job
title: Ändern der Zeitplandetails für einen Masterauftrag
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: f5414451-4d8e-464b-bd9e-f2b70c6899b3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: a49821ba65ede149baf2ce4aeaa54724dd796421
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765834"
---
# <a name="change-the-scheduling-details-for-a-sql-server-agent-master-job"></a>Change the Scheduling Details for a SQL Server Agent Master Job

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie Sie Zeitplandetails mithilfe von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [!INCLUDE[tsql](../../includes/tsql-md.md)]ändern.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Einschränkungen  
Ein Masterauftrag für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent kann nicht gleichzeitig lokale Server und Remoteserver als Ziel haben.  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
  
#### <a name="permissions"></a><a name="Permissions"></a>Berechtigungen  
Sie können nur Aufträge ändern, die in Ihrem Besitz sind, es sei denn, Sie sind ein Mitglied der festen Serverrolle **sysadmin** . Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-change-the-scheduling-details-for-a-job-definition"></a>So ändern Sie die Zeitplandetails für eine Auftragsdefinition  
  
1. Klicken Sie im **Objekt-Explorer** auf das Pluszeichen, um den Server zu erweitern, in dem der Auftrag mit dem zu bearbeitenden Zeitplan enthalten ist.  
  
2. Klicken Sie auf das Pluszeichen, um **SQL Server-Agent** zu erweitern.  
  
3. Klicken Sie auf das Pluszeichen, um den Ordner **Aufträge** zu erweitern.  
  
4. Klicken Sie mit der rechten Maustaste auf den Auftrag, dessen Zeitplan Sie bearbeiten möchten, und wählen Sie **Eigenschaften** aus.  
  
5. Klicken Sie im Dialogfeld **Auftragseigenschaften >** _Auftragsname_ unter **Seite auswählen** auf die Option **Zeitpläne**. Weitere Informationen zu den verfügbaren Optionen auf dieser Seite finden Sie unter [Auftragseigenschaften – Neuer Auftrag &#40;Seite „Zeitpläne“&#41;](../../ssms/agent/job-properties-new-job-schedules-page.md).  
  
6. Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Verwenden von Transact-SQL  
  
#### <a name="to-change-the-scheduling-details-for-a-job-definition"></a>So ändern Sie die Zeitplandetails für eine Auftragsdefinition
  
1. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3. Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- changes the enabled status of the NightlyJobs schedule to 0
    -- and sets the owner to terrid.
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_update_schedule  
        @name = 'NightlyJobs',  
        @enabled = 0,  
        @owner_login_name = 'terrid' ;  
    GO  
    ```  
  
Weitere Informationen finden Sie unter [sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md).