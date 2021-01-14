---
description: sys.dm_exec_query_memory_grants (Transact-SQL)
title: sys.dm_exec_query_memory_grants (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_memory_grants_TSQL
- sys.dm_exec_query_memory_grants
- sys.dm_exec_query_memory_grants_TSQL
- dm_exec_query_memory_grants
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_memory_grants dynamic management view
ms.assetid: 2c417747-2edd-4e0d-8a9c-e5f445985c1a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 147bf57a568460c0b1df68fc4e232432bef3b834
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170282"
---
# <a name="sysdm_exec_query_memory_grants-transact-sql"></a>sys.dm_exec_query_memory_grants (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu allen Abfragen zurück, die angefordert haben und auf eine Arbeitsspeicher Zuweisung warten oder eine Speicherzuweisung erhalten haben. Abfragen, die keine Arbeitsspeicher Zuweisung erfordern, werden in dieser Ansicht nicht angezeigt. Beispielsweise verfügen Sortier-und HashJoin-Vorgänge über Arbeitsspeicher Zuweisungen für die Abfrage Ausführung, während Abfragen ohne **Order by** -Klausel keine Arbeitsspeicher Zuweisung haben.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]können dynamische Verwaltungssichten keine Informationen verfügbar machen, die sich auf die Datenbankkapselung auswirken würden oder die sich auf andere Datenbanken beziehen, auf die der Benutzer Zugriff hat. Um zu vermeiden, dass diese Informationen verfügbar gemacht werden, wird jede Zeile, die Daten enthält, die nicht zum verbundenen Mandanten gehören, herausgefiltert. Darüber hinaus werden die Werte in den Spalten **scheduler_id**, **wait_order** **pool_id**, **group_id** gefiltert. der Spaltenwert wird auf NULL festgelegt.  
  
> [!NOTE]  
> Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_exec_query_memory_grants**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|ID (SPID) der Sitzung, in der die Abfrage ausgeführt wird.|  
|**request_id**|**int**|ID der Anforderung. Ist im Kontext der Sitzung eindeutig.|  
|**scheduler_id**|**int**|ID des Zeitplanungsmoduls, der diese Abfrage plant.|  
|**DOP**|**smallint**|Grad an Parallelität für diese Abfrage.|  
|**request_time**|**datetime**|Datum und Uhrzeit, zu der die Abfrage die Arbeitsspeicherzuweisung angefordert hat.|  
|**grant_time**|**datetime**|Datum und Uhrzeit, zu der die Arbeitsspeicherzuweisung für die Abfrage erfolgt ist. NULL, wenn noch kein Arbeitsspeicher zugewiesen wurde.|  
|**requested_memory_kb**|**bigint**|Insgesamt angeforderter Arbeitsspeicher in Kilobytes.|  
|**granted_memory_kb**|**bigint**|Insgesamt tatsächlich zugewiesener Arbeitsspeicher in Kilobytes. Kann NULL sein, wenn noch kein Arbeitsspeicher zugewiesen wurde. Normalerweise sollte dieser Wert mit **requested_memory_kb** übereinstimmen. Für die Indexerstellung wird möglicherweise vom Server bei Bedarf weiterer Arbeitsspeicher über den ursprünglich zugewiesenen hinaus zugelassen.|  
|**required_memory_kb**|**bigint**|Minimaler Arbeitsspeicher in Kilobyte, der erforderlich ist, um diese Abfrage auszuführen. **requested_memory_kb** ist gleich oder größer als dieser Betrag.|  
|**used_memory_kb**|**bigint**|Der zu diesem Zeitpunkt verwendete physische Arbeitsspeicher in Kilobytes.|  
|**max_used_memory_kb**|**bigint**|Der bis zu diesem Zeitpunkt verwendete maximale physische Arbeitsspeicher in Kilobytes.|  
|**query_cost**|**float**|Die geschätzten Abfragekosten.|  
|**timeout_sec**|**int**|Timeout in Sekunden, nach dem die Abfrage die Anforderung der Arbeitsspeicherzuweisung aufgibt.|  
|**resource_semaphore_id**|**smallint**|Nicht eindeutige ID des Ressourcensemaphors, auf das die Abfrage wartet.<br /><br /> **Hinweis:** Diese ID ist in Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die älter sind als, eindeutig [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . Diese Änderung kann die Abfrageausführung bei der Problembehandlung beeinflussen. Weitere Informationen finden Sie im Abschnitt "Hinweise" weiter unten in diesem Thema.|  
|**queue_id**|**smallint**|ID der Warteschlange, in der die Abfrage auf Arbeitsspeicherzuweisungen wartet. NULL, wenn der Arbeitsspeicher bereits zugewiesen wurde.|  
|**wait_order**|**int**|Die sequenzielle Position wartender Abfragen in der Warteschlange mit der angegebenen **queue_id**. Dieser Wert kann sich für eine bestimmte Abfrage ändern, wenn andere Abfragen Arbeitsspeicher Zuweisungen erhalten oder ein Timeout auftritt. NULL, wenn bereits Arbeitsspeicher zugewiesen wurde.|  
|**is_next_candidate**|**bit**|Kandidat für die nächste Arbeitsspeicherzuweisung.<br /><br /> 1 = Ja<br /><br /> 0 = Nein<br /><br /> NULL = Arbeitsspeicher wurde bereits zugewiesen|  
|**wait_time_ms**|**bigint**|Wartezeit in Millisekunden. NULL, wenn der Arbeitsspeicher bereits zugewiesen wurde.|  
|**plan_handle**|**varbinary(64)**|Bezeichner für diesen Abfrageplan. Verwenden Sie **sys.dm_exec_query_plan**, um den tatsächlichen XML-Plan zu extrahieren.|  
|**sql_handle**|**varbinary(64)**|Bezeichner für den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Text dieser Abfrage. Verwenden Sie **sys.dm_exec_sql_text**, um den tatsächlichen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Text abzurufen.|  
|**group_id**|**int**|ID für die Arbeitsauslastungsgruppe, in der diese Abfrage ausgeführt wird.|  
|**pool_id**|**int**|ID des Ressourcenpools, zu dem die Arbeitsauslastungsgruppe gehört.|  
|**is_small**|**tinyint**|Der Wert 1 gibt an, dass diese Zuweisung das kleine Ressourcensemaphor verwendet. Der Wert 0 gibt an, dass ein normales Semaphor verwendet wird.|  
|**ideal_memory_kb**|**bigint**|Größe der Arbeitsspeicherzuweisung in Kilobyte (KB), um alles in den physischen Speicher aufzunehmen. Dieser Wert basiert auf der Kardinalitätsschätzung.|  
|**pdw_node_id**|**int**|Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.<br /><br /> **Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
|**reserved_worker_count**|**bigint**|Anzahl reservierter [Arbeitsthreads](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling).<br /><br />**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|**used_worker_count**|**bigint**|Anzahl von [Arbeitsthreads](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) , die zu diesem Zeitpunkt verwendet werden.<br /><br />**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**max_used_worker_count**|**bigint**|Die maximale Anzahl von [Arbeitsthreads](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) , die bis zu diesem Zeitpunkt verwendet werden.<br /><br />**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**reserved_node_bitmap**|**bigint**|Bitmap von NUMA-Knoten, bei denen [Arbeitsthreads](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) reserviert sind.<br /><br />**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ist die Berechtigung `VIEW DATABASE STATE` in der Datenbank erforderlich.   
   
## <a name="remarks"></a>Bemerkungen  
 Ein typisches Debugszenario für ein Abfragetimeout sieht folgendermaßen aus:  
  
-   Überprüfen Sie den Arbeitsspeicherstatus im Gesamtsystem mithilfe von [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md), [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) und verschiedenen Leistungsindikatoren.  
  
-   Überprüfen Sie die Arbeitsspeicherreservierungen für die Abfrageausführung in **sys.dm_os_memory_clerks**, wobei `type = 'MEMORYCLERK_SQLQERESERVATIONS'` ist.  
  
-   Überprüfen Sie, ob Abfragen, die auf <sup>1</sup> warten, mit **sys.dm_exec_query_memory_grants**.  
  
    ```sql  
    --Find all queries waiting in the memory queue  
    SELECT * FROM sys.dm_exec_query_memory_grants where grant_time is null  
    ```  
    
    <sup>1</sup> In diesem Szenario ist der Wartetyp in der Regel RESOURCE_SEMAPHORE. Weitere Informationen finden Sie unter [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). 
  
-   Suchen Sie Cache nach Abfragen mit Arbeitsspeicher Zuweisungen mithilfe [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) und [sys.dm_exec_query_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
  
    ```sql  
    -- retrieve every query plan from the plan cache  
    USE master;  
    GO  
    SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
    GO  
    ```  
  
-   Untersuchen Sie arbeitsspeicherintensive Abfragen mithilfe von [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).  
  
    ```sql  
    --Find top 5 queries by average CPU time  
    SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
      plan_handle, query_plan   
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
    ORDER BY total_worker_time/execution_count DESC;  
    GO  
    ```  
  
-   Untersuchen Sie beim Verdacht auf eine Endlosabfrage den Showplan in [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) und den Batchtext in [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 Abfragen, die dynamische Verwaltungs Sichten verwenden, die-oder-Aggregate enthalten, `ORDER BY` können die Arbeitsspeicher Nutzung erhöhen und so zu dem Problem beitragen, das Sie beheben.  
  
 Mit der Ressourcenkontrollen-Funktion kann ein Datenbankadministrator Serverressourcen auf Ressourcenpools verteilen, bis zu maximal 64 Pools. Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] verhält sich jeder Pool wie eine kleine unabhängige Serverinstanz und erfordert 2 Semaphoren. Die Anzahl der Zeilen, die von **sys.dm_exec_query_resource_semaphores** zurückgegeben werden, kann bis zu 20-mal mehr als die Zeilen sein, die in zurückgegeben werden [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_exec_query_resource_semaphores &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)     
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
 [Handbuch zur Thread- und Taskarchitektur](../../relational-databases/thread-and-task-architecture-guide.md)   
  
