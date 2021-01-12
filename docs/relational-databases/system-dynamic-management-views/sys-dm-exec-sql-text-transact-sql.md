---
description: sys.dm_exec_sql_text (Transact-SQL)
title: sys.dm_exec_sql_text (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_sql_text
- sys.dm_exec_sql_text
- sys.dm_exec_sql_text_TSQL
- dm_exec_sql_text_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sql_text dynamic management function
ms.assetid: 61b8ad6a-bf80-490c-92db-58dfdff22a24
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ae67ee26b6b44951ae0c798de4461dc846b29352
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098920"
---
# <a name="sysdm_exec_sql_text-transact-sql"></a>sys.dm_exec_sql_text (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt den Text des SQL-Batches zurück, der durch die angegebene *sql_handle* identifiziert wird. Diese Tabellenwertfunktion ersetzt die Systemfunktion **fn_get_sql**.  
  
 
## <a name="syntax"></a>Syntax  
  
```  
sys.dm_exec_sql_text(sql_handle | plan_handle)  
```  
  
## <a name="arguments"></a>Argumente  
*sql_handle*  
Ein Token, das einen Batch eindeutig identifiziert, der ausgeführt wurde oder gerade ausgeführt wird. *sql_handle* ist vom Datentyp **varbinary (64)**. 

Der *sql_handle* kann aus den folgenden dynamischen Verwaltungs Objekten abgerufen werden:  
  
-   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
  
-   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
  
-   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  
  
*plan_handle*  
Ein Token, das einen Abfrage Ausführungsplan für einen Batch eindeutig identifiziert, der ausgeführt wurde und dessen Plan sich im Plancache befindet oder gerade ausgeführt wird. *plan_handle* ist **varbinary(64)**   

*plan_handle* kann aus den folgenden dynamischen Verwaltungsobjekten abgerufen werden:    
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|ID der Datenbank.<br /><br /> Für Ad-hoc-Anweisungen und vorbereitete SQL-Anweisungen, die ID der Datenbank, in der die Anweisungen kompiliert wurden.|  
|**objectid**|**int**|ID des Objekts.<br /><br /> Dieser Wert ist für Ad-hoc-Anweisungen und vorbereitete SQL-Anweisungen NULL.|  
|**Zahl**|**smallint**|Für eine nummerierte gespeicherte Prozedur gibt diese Spalte die Nummer der gespeicherten Prozedur zurück. Weitere Informationen finden Sie unter [sys.numbered_procedures &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md).<br /><br /> Dieser Wert ist für Ad-hoc-Anweisungen und vorbereitete SQL-Anweisungen NULL.|  
|**.**|**bit**|1 = Der SQL-Text ist verschlüsselt.<br /><br /> 0 = Der SQL-Text ist nicht verschlüsselt.|  
|**text**|**nvarchar (max** **)**|Text der SQL-Abfrage.<br /><br /> Der Wert ist für verschlüsselte Objekte NULL.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die `VIEW SERVER STATE`-Berechtigung auf dem Server.  
  
## <a name="remarks"></a>Bemerkungen  
Bei Ad-hoc-Abfragen sind die SQL-Handles Hashwerte auf der Grundlage des SQL-Texts, der an den Server übermittelt wird. Sie können aus jeder Datenbank stammen. 

Für Datenbankobjekte, z. B. gespeicherte Prozeduren, Trigger oder Funktionen, werden die SQL-Handles von der Datenbank-ID, Objekt-ID und Objektnummer abgeleitet. 

Das Plan Handle ist ein Hashwert, der aus dem kompilierten Plan des gesamten Batches abgeleitet ist. 

> [!NOTE]
> **DBID** kann nicht aus *sql_handle* für Ad-hoc-Abfragen ermittelt werden. Um **DBID** für Ad-hoc-Abfragen zu ermitteln, verwenden Sie stattdessen *plan_handle* .
  
## <a name="examples"></a>Beispiele 

### <a name="a-conceptual-example"></a>A. Konzeptionelles Beispiel
Im folgenden finden Sie ein einfaches Beispiel zur Veranschaulichung der Übergabe eines **sql_handle** entweder direkt oder mit **CROSS APPLY**.
  1.  Aktivität erstellen.  
Führen Sie den folgenden T-SQL-Code in einem neuen Abfragefenster in aus [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .   
      ```sql
      -- Identify current spid (session_id)
      SELECT @@SPID;
      GO
  
      -- Create activity
        WAITFOR DELAY '00:02:00';
      ```
      
  2.  Verwenden von **CROSS APPLY**.  
    Die sql_handle von **sys.dm_exec_requests** wird mithilfe von **CROSS APPLY** an **sys.dm_exec_sql_text** übermittelt. Öffnen Sie ein neues Abfragefenster, und übergeben Sie die in Schritt 1 identifizierte SPID. In diesem Beispiel ist die SPID `59` .

        ```sql
        SELECT t.*
        FROM sys.dm_exec_requests AS r
        CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t
        WHERE session_id = 59 -- modify this value with your actual spid
         ```      
 
  2.  Direktes übergeben von **sql_handle** .  
Rufen Sie die **sql_handle** von **sys.dm_exec_requests** ab. Übergeben Sie dann die **sql_handle** direkt an **sys.dm_exec_sql_text**. Öffnen Sie ein neues Abfragefenster, und übergeben Sie die in Schritt 1 identifizierte SPID an **sys.dm_exec_requests**. In diesem Beispiel ist die SPID `59` . Übergeben Sie dann die zurückgegebene **sql_handle** als Argument an **sys.dm_exec_sql_text**.

        ```sql
        -- acquire sql_handle
        SELECT sql_handle FROM sys.dm_exec_requests WHERE session_id = 59  -- modify this value with your actual spid
        
        -- pass sql_handle to sys.dm_exec_sql_text
        SELECT * FROM sys.dm_exec_sql_text(0x01000600B74C2A1300D2582A2100000000000000000000000000000000000000000000000000000000000000) -- modify this value with your actual sql_handle
         ```      
    
  
### <a name="b-obtain-information-about-the-top-five-queries-by-average-cpu-time"></a>B. Abrufen von Informationen zu den fünf Abfragen mit der höchsten durchschnittlichen CPU-Zeit  
 Im folgenden Beispiel wird der Text der SQL-Anweisung und die durchschnittliche CPU-Zeit für die fünf Abfragen mit der höchsten durchschnittlichen CPU-Zeit zurückgegeben.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
    SUBSTRING(st.text, (qs.statement_start_offset/2)+1,   
        ((CASE qs.statement_end_offset  
          WHEN -1 THEN DATALENGTH(st.text)  
         ELSE qs.statement_end_offset  
         END - qs.statement_start_offset)/2) + 1) AS statement_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
ORDER BY total_worker_time/execution_count DESC;  
```  
  
### <a name="c-provide-batch-execution-statistics"></a>C. Bereitstellen von Statistiken zur Batch Ausführung  
 Im folgenden Beispiel wird der Text von SQL-Abfragen zurückgegeben, die in Batches ausgeführt werden. Außerdem werden statistische Informationen zu den Abfragen bereitgestellt.  
  
```sql  
SELECT s2.dbid,   
    s1.sql_handle,    
    (SELECT TOP 1 SUBSTRING(s2.text,statement_start_offset / 2+1 ,   
      ( (CASE WHEN statement_end_offset = -1   
         THEN (LEN(CONVERT(nvarchar(max),s2.text)) * 2)   
         ELSE statement_end_offset END)  - statement_start_offset) / 2+1))  AS sql_statement,  
    execution_count,   
    plan_generation_num,   
    last_execution_time,     
    total_worker_time,   
    last_worker_time,   
    min_worker_time,   
    max_worker_time,  
    total_physical_reads,   
    last_physical_reads,   
    min_physical_reads,    
    max_physical_reads,    
    total_logical_writes,   
    last_logical_writes,   
    min_logical_writes,   
    max_logical_writes    
FROM sys.dm_exec_query_stats AS s1   
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS s2    
WHERE s2.objectid is null   
ORDER BY s1.sql_handle, s1.statement_start_offset, s1.statement_end_offset;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_cursors &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)   
 [sys.dm_exec_xml_handles &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [Verwenden von Apply](../../t-sql/queries/from-transact-sql.md#using-apply)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  

