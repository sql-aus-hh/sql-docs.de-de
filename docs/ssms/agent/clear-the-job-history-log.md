---
description: Clear the Job History Log
title: Clear the Job History Log
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- clearing job history log
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: 34b9398a-c409-4040-8ea1-0deceb18f961
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9d48a3ea6b2660504b613b402f024c8f42ae7557
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766295"
---
# <a name="clear-the-job-history-log"></a>Clear the Job History Log
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In diesem Thema wird beschrieben, wie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] oder SQL Server Management Objects der Inhalt des Verlaufsprotokolls für [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Aufträge gelöscht werden kann.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Vorbereitungen  
  
### <a name="security"></a><a name="Security"></a>Sicherheit  
Ausführliche Informationen finden Sie unter [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-clear-the-job-history-log"></a>So löschen Sie das Auftragsverlaufsprotokoll  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]her, und erweitern Sie dann diese Instanz.  
  
2.  Erweitern Sie **SQL Server-Agent**, und klicken Sie auf **Aufträge**.  
  
3.  Klicken Sie mit der rechten Maustaste auf einen Auftrag, und klicken Sie dann auf **Verlauf anzeigen**.  
  
4.  Wählen Sie im **Protokolldatei-Viewer** den Auftrag aus, dessen Verlauf gelöscht werden soll, und führen Sie dann einen der folgenden Schritte aus:  
  
    -   Klicken Sie auf **Löschen**, und klicken Sie dann im Dialogfeld **Verlauf löschen** auf **Gesamten Verlauf löschen** . Sie können wahlweise den gesamten Auftragsverlauf löschen oder auch nur den Verlauf, der vor einem bestimmten Datum angefallen ist. Soll der gesamte Auftragsverlauf gelöscht werden, klicken Sie auf **Gesamten Verlauf löschen**. Um nur ältere Einträge im Auftragsverlauf zu löschen, klicken Sie auf **Verlauf löschen vor**, und geben Sie das gewünschte Datum an.  
  
    -   Klicken Sie auf **Auftragsstatus** , wenn Sie das Verlaufsprotokoll eines Multiserverauftrags löschen möchten. Klicken Sie auf **Auftrag** und auf einen Auftragsnamen, und klicken Sie dann auf **Remoteauftragsverlauf anzeigen**.  
  
5.  Klicken Sie auf **Löschen**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Verwenden von Transact-SQL  
  
#### <a name="to-clear-the-job-history-log"></a>So löschen Sie das Auftragsverlaufsprotokoll  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde_md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    -- example removes the history for a job named NightlyBackups.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_purge_jobhistory  
        @job_name = N'NightlyBackups' ;  
    GO  
    ```  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Verwendung von SQL Server Management Objects  
**So löschen Sie das Auftragsverlaufsprotokoll**  
  
Verwenden Sie die **PurgeJobHistory** -Methode der **JobServer** -Klasse in einer Programmiersprache Ihrer Wahl, z. B. Visual Basic, Visual C# oder PowerShell. Weitere Informationen finden Sie unter [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
