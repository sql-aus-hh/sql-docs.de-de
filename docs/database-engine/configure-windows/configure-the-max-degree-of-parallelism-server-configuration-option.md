---
title: Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“ | Microsoft-Dokumentation
description: Informationen zur Option „max degree of parallelism“ (MAXDOP) Hier erfahren Sie, wie Sie diese Option verwenden, um die Anzahl der Prozessoren einzuschränken, die von SQL Server bei der Ausführung paralleler Pläne verwendet werden.
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parallel queries [SQL Server]
- processors [SQL Server], parallel queries
- number of processors for parallel queries
- max degree of parallelism option
- MaxDop
ms.assetid: 86b65bf1-a6a1-4670-afc0-cdfad1558032
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.openlocfilehash: 7b0e4b8abf21d918e7d4d627c7ed82d5507394ec
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171062"
---
# <a name="configure-the-max-degree-of-parallelism-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Artikel wird beschrieben, wie Sie die Serverkonfigurationsoption **Max. Grad an Parallelität (MAXDOP)** in SQL Server mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)] konfigurieren. Wenn eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer mit mehreren Mikroprozessoren oder CPUs ausgeführt wird, erkennt die [!INCLUDE[ssde_md](../../includes/ssde_md.md)], ob Parallelität verwendet werden kann. Mit dem Grad der Parallelität wird die Anzahl der Prozessoren festgelegt, die zur Ausführung einer einzelnen Anweisung für jede Ausführung paralleler Pläne verwendet werden. Mithilfe der Option **Max. Grad an Parallelität** kann die Anzahl der Prozessoren beschränkt werden, die bei der Ausführung paralleler Pläne verwendet werden. Weitere Informationen zu den durch den **maximalen Grad an Parallelität (MAXDOP)** festgelegten Grenzwert finden Sie im Abschnitt [Überlegungen](#Considerations) auf dieser Seite. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berücksichtigt die Ausführung paralleler Pläne für Abfragen, DDL-Indizierungsvorgänge (Data Definition Language, DDL), parallele Einfügevorgänge, Onlineausführung von ALTER COLUMN, parallele Sammlung von Statistiken sowie die statische und keysetgesteuerte Cursorauffüllung.

> [!NOTE]
> Mit [!INCLUDE [sssqlv15-md](../../includes/sssqlv15-md.md)] wurden automatische Empfehlungen zum Festlegen der MAXDOP-Serverkonfigurationsoption während des Installationsvorgangs basierend auf der Anzahl der verfügbaren Prozessoren eingeführt. Auf der Setupbenutzeroberfläche können Sie entweder die empfohlenen Einstellungen übernehmen oder Ihren eigenen Wert eingeben. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Seite „MaxDOP“](../../sql-server/install/instance-configuration.md#maxdop).

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="considerations"></a><a name="Considerations"></a> Weitere Überlegungen  
-   Diese Option ist eine erweiterte Option und sollte ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Experten geändert werden.  

-   Wenn die Option Affinity Mask nicht auf den Standardwert festgelegt ist, steht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Systemen mit symmetrischem Multiprocessing (SMP) möglicherweise nur eine beschränkte Anzahl an Prozessoren zur Verfügung. 
  
-   Durch das Festlegen des maximalen Parallelitätsgrads (MAXDOP) auf 0 (null) kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle verfügbaren Prozessoren verwenden (bis maximal 64 Prozessoren). Dies ist jedoch in den meisten Fällen nicht der empfohlene Wert. Weitere Informationen zu den empfohlenen Werten für den maximalen Parallelitätsgrad finden Sie im Abschnitt [Empfehlungen](#Recommendations) auf dieser Seite.

-   Legen Sie **Max. Grad an Parallelität** auf 1 fest, um das Generieren paralleler Pläne zu unterdrücken. Legen Sie den Wert auf eine Zahl zwischen 1 und 32767 fest, um die maximale Anzahl von Prozessorkernen anzugeben, die während einer einzelnen Abfrageausführung verwendet werden können. Wenn ein Wert angegeben wird, der über der Anzahl der verfügbaren Prozessoren liegt, wird die tatsächliche Anzahl der Prozessoren verwendet. Verfügt der Computer nur über einen Prozessor, wird der Wert von **Max. Grad an Parallelität** ignoriert.  

-   Der Grenzwert für den maximalen Parallelitätsgrad wird [taskbezogen](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) festgelegt. Es handelt sich nicht um einen [anforderungs](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)- oder abfragebezogenen Grenzwert. Dies bedeutet, dass eine einzelne Anforderung während einer parallelen Abfrageausführung mehrere Tasks bis zum Grenzwert MAXDOP erzeugen kann, wobei jeder Task einen Worker und einen Planer nutzt. Weitere Informationen finden Sie im [Handbuch zur Thread- und Taskarchitektur](../../relational-databases/thread-and-task-architecture-guide.md) im Abschnitt *Planen von parallelen Tasks*. 
  
-   Sie können den Serverkonfigurationswert für den maximalen Parallelitätsgrad außer Kraft setzen:
    -   Verwenden Sie auf Abfrageebene hierfür den [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md) für **MAXDOP**.     
    -   Auf Datenbankebene verwenden Sie die [auf die Datenbank beschränkte Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) für **MAXDOP**.
    -   Auf Arbeitsauslastungsebene nutzen Sie die **Konfigurationsoption für die Resource Governor-Arbeitsauslastungsgruppe** für [MAX_DOP](../../t-sql/statements/create-workload-group-transact-sql.md).

-   Indizierungsoperationen, bei denen ein Index erstellt oder neu aufgebaut wird bzw. an deren Ende ein gruppierter Index steht, können ressourcenintensiv sein. In Indizierungsoperationen kann der Wert "Max. Grad an Parallelität" überschrieben werden; geben Sie hierzu die Indexoption MAXDOP in der Indexanweisung an. Der Wert MAXDOP wird zur Ausführungszeit auf die Anweisung angewendet und wird nicht in den Metadaten für den Index gespeichert. Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
-   Neben Abfragen und Indexoperationen steuert diese Option auch die Parallelität von DBCC CHECKTABLE, DBCC CHECKDB und DBCC CHECKFILEGROUP. Sie können Pläne für die parallele Ausführung für diese Anweisungen deaktivieren, und zwar mithilfe des Ablaufverfolgungsflags 2528. Weitere Informationen finden Sie unter [Ablaufverfolgungsflags &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

###  <a name="recommendations"></a><a name="Recommendations"></a><a name="Guidelines"></a>Empfehlungen  
Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] werden beim Starten des Diensts standardmäßig automatisch Soft-NUMA-Knoten erstellt, wenn [!INCLUDE[ssde_md](../../includes/ssde_md.md)] beim Startup mehr als acht physische Kerne pro NUMA-Knoten oder Socket erkennt. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] platziert logische Prozessoren aus dem gleichen physischen Kern in verschiedene Soft-NUMA-Knoten. Die Empfehlungen in der folgenden Tabelle sollen alle Arbeitsthreads einer parallelen Abfrage innerhalb des gleichen NUMA-Knotens beibehalten. Dies verbessert die Leistung der Abfragen und die Verteilung von Arbeitsthreads in allen NUMA-Knoten für die Workload. Weitere Informationen finden Sie unter [Soft-NUMA](../../database-engine/configure-windows/soft-numa-sql-server.md).

Verwenden Sie ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] die folgenden Richtlinien beim Konfigurieren des Serverkonfigurationswerts **Max. Grad an Parallelität**:

|Serverkonfiguration|Anzahl der Prozessoren|Anleitungen|
|----------------|-----------------|-----------------|
|Server mit einzelnem NUMA-Knoten|Weniger als oder gleich 8 logische Prozessoren|Belassen Sie MAXDOP bei # oder weniger logischen Prozessoren|
|Server mit einzelnem NUMA-Knoten|Mehr als 8 logische Prozessoren|Belassen Sie MAXDOP bei 8|
|Server mit mehreren NUMA-Knoten|Weniger als oder gleich 16 logische Prozessoren pro NUMA-Knoten|Belassen Sie MAXDOP bei # oder weniger logischen Prozessoren pro NUMA-Knoten|
|Server mit mehreren NUMA-Knoten|Mehr als 16 logische Prozessoren pro NUMA-Knoten|Sorgen Sie dafür, dass MAXDOP der Hälfte der logischen Prozessoren pro NUMA-Knoten mit einem MAX-Wert von 16 entspricht.|
  
> [!NOTE]
> Der NUMA-Knoten in der obigen Tabelle bezieht sich auf automatisch von [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höheren Versionen erstellte Soft-NUMA-Knoten oder auf hardwarebasierte NUMA-Knoten, wenn Soft-NUMA deaktiviert wurde.   
>  Verwenden Sie dieselben Richtlinien, wenn Sie die Option „Max. Grad an Parallelität“ für Resource Governor-Arbeitsauslastungsgruppen festlegen. Weitere Informationen finden Sie unter [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md).
  
Verwenden Sie von **bis** die folgenden Richtlinien beim Konfigurieren des Serverkonfigurationswerts [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]Max. Grad an Parallelität[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]:

|Serverkonfiguration|Anzahl der Prozessoren|Anleitungen|
|----------------|-----------------|-----------------|
|Server mit einzelnem NUMA-Knoten|Weniger als oder gleich 8 logische Prozessoren|Belassen Sie MAXDOP bei # oder weniger logischen Prozessoren|
|Server mit einzelnem NUMA-Knoten|Mehr als 8 logische Prozessoren|Belassen Sie MAXDOP bei 8|
|Server mit mehreren NUMA-Knoten|Weniger als oder gleich 8 logische Prozessoren pro NUMA-Knoten|Belassen Sie MAXDOP bei # oder weniger logischen Prozessoren pro NUMA-Knoten|
|Server mit mehreren NUMA-Knoten|Mehr als 8 logische Prozessoren pro NUMA-Knoten|Belassen Sie MAXDOP bei 8|
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>So konfigurieren Sie die Option Max. Grad an Parallelität  
  
1.  Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den **Erweitert** -Knoten.  
  
3.  Wählen Sie im Feld **Max. Grad an Parallelität** die maximale Anzahl der Prozessoren aus, die bei der Ausführung paralleler Pläne verwendet werden sollen.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>So konfigurieren Sie die Option Max. Grad an Parallelität  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um die Option `max degree of parallelism` auf `16`festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO   
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
EXEC sp_configure 'max degree of parallelism', 16;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-max-degree-of-parallelism-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Max. Grad an Parallelität“  
 Die Einstellung tritt ohne Neustarten des Servers sofort in Kraft.  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)        
 [Affinitätsmaske (Serverkonfigurationsoption)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)      
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Leitfaden zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#DOP)       
 [Handbuch zur Thread- und Taskarchitektur](../../relational-databases/thread-and-task-architecture-guide.md)    
 [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md)    
 [Abfragehinweise (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)     
 [Festlegen von Indexoptionen](../../relational-databases/indexes/set-index-options.md)     

## <a name="next-steps"></a>Nächste Schritte

[RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)
[Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)
