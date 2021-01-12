---
description: sys.query_store_wait_stats (Transact-SQL)
title: sys.query_store_wait_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d3df8f87d73ac8ca25fe2e7e83d7bbc3d0da01f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097964"
---
# <a name="sysquery_store_wait_stats-transact-sql"></a>sys.query_store_wait_stats (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Enthält Informationen zu den warte Informationen für die Abfrage.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|Der Bezeichner der Zeile, die die Warte Statistik für die plan_id, runtime_stats_interval_id execution_type und wait_category darstellt. Er ist nur für die letzten Lauf Zeit Statistik Intervalle eindeutig. Für das derzeit aktive Intervall gibt es möglicherweise mehrere Zeilen, die Warte Statistiken für den Plan darstellen, auf den von plan_id verwiesen wird, wobei der Ausführungstyp execution_type und die durch wait_category dargestellte warte Kategorie ist. In der Regel stellt eine Zeile eine warte Statistik dar, die auf den Datenträger geleert wird, während andere (n) den Speicher internen Status darstellen. Um den tatsächlichen Zustand für jedes Intervall zu erhalten, müssen Sie daher Metriken aggregieren, indem Sie plan_id, runtime_stats_interval_id, execution_type und wait_category gruppieren. |  
|**plan_id**|**bigint**|Fremdschlüssel. Joins mit [sys.query_store_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Fremdschlüssel. Joins mit [sys.query_store_runtime_stats_interval &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**wait_category**|**tinyint**|Warte Typen werden mithilfe der nachfolgenden Tabelle kategorisiert, und dann wird die Wartezeit über diese warte Kategorien aggregiert. Für unterschiedliche warte Kategorien sind unterschiedliche nachfolgenden Analysen erforderlich, um das Problem zu beheben. warte Typen aus der gleichen Kategorie führen jedoch zu ähnlichen Problemen bei der Problembehandlung, und das Bereitstellen der betroffenen Abfrage zusätzlich zu den Wartezeiten ist der fehlende Teil, um die Mehrzahl dieser Untersuchungen erfolgreich abzuschließen.|
|**wait_category_desc**|**nvarchar(128)**|Eine Textbeschreibung des Felds "Warte Kategorie" finden Sie in der folgenden Tabelle.|
|**execution_type**|**tinyint**|Bestimmt den Typ der Abfrage Ausführung:<br /><br /> 0-reguläre Ausführung (erfolgreich abgeschlossen)<br /><br /> 3-vom Client initiierte Ausführung abgebrochen<br /><br /> 4-Ausnahme Abbruch abgebrochen|  
|**execution_type_desc**|**nvarchar(128)**|Textbeschreibung des Felds "Ausführungstyp":<br /><br /> 0-regulär<br /><br /> 3-abgebrochen<br /><br /> 4-Ausnahme|  
|**total_query_wait_time_ms**|**bigint**|Gesamt `CPU wait` Zeit für den Abfrageplan innerhalb des Aggregations Intervalls und der Warte Kategorie (in Millisekunden angegeben).|
|**avg_query_wait_time_ms**|**float**|Durchschnittliche Wartezeit für den Abfrageplan pro Ausführung innerhalb des Aggregations Intervalls und der Warte Kategorie (in Millisekunden angegeben).|
|**last_query_wait_time_ms**|**bigint**|Die Dauer der letzten Wartezeit für den Abfrageplan innerhalb des Aggregations Intervalls und der Wartezeit (in Millisekunden angegeben).|
|**min_query_wait_time_ms**|**bigint**|Minimale `CPU wait` Zeit für den Abfrageplan innerhalb des Aggregations Intervalls und der Wartezeit (in Millisekunden angegeben).|
|**max_query_wait_time_ms**|**bigint**|Maximale `CPU wait` Zeit für den Abfrageplan innerhalb des Aggregations Intervalls und der Warte Kategorie (in Millisekunden angegeben).|
|**stdev_query_wait_time_ms**|**float**|`Query wait` Dauer der Standardabweichung für den Abfrageplan innerhalb des Aggregations Intervalls und der Wartezeit (in Millisekunden angegeben).|

## <a name="wait-categories-mapping-table"></a>Warte Kategorien-Mapping-Tabelle

"%" wird als Platzhalter verwendet.
  
|Ganzzahliger Wert|Warte Kategorie|Warte Typen sind in der Kategorie enthalten.|  
|-----------------|---------------|-----------------|  
|**0**|**Unbekannt**|Unbekannt |  
|**1**|**CPU**|SOS_SCHEDULER_YIELD|
|**2**|**Arbeits Thread**|THREADPOOL|
|**3**|**Sperre**|LCK_M_%|
|**4**|**Latch**|LATCH_%|
|**5**|**Pufferlatch**|PAGELATCH_%|
|**6**|**Puffer-IO**|PAGEIOLATCH_%|
|**7**|**Kompilierung** _|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|_ *8**|**SQL CLR**|CLR%, SQLCLR%|
|**9**|**Spiegelung**|Dbmirror%|
|**10**|**Transaktion**|xact%, DTC%, TRAN_MARKLATCH_%, MSQL_XACT_%, TRANSACTION_MUTEX|
|**11**|**Gesch**|SLEEP_%, LAZYWRITER_SLEEP, SQLTRACE_BUFFER_FLUSH, SQLTRACE_INCREMENTAL_FLUSH_SLEEP, SQLTRACE_WAIT_ENTRIES, FT_IFTS_SCHEDULER_IDLE_WAIT, XE_DISPATCHER_WAIT, REQUEST_FOR_DEADLOCK_SEARCH, LOGMGR_QUEUE, ONDEMAND_TASK_QUEUE CHECKPOINT_QUEUE, XE_TIMER_EVENT|
|**12**|**PreEmptive**|PREEMPTIVE_%|
|**13**|**Service Broker**|BROKER_% **(aber nicht BROKER_RECEIVE_WAITFOR)**|
|**14**|**Tran Log IO**|logmgr, logbuffer, LOGMGR_RESERVE_APPEND, LOGMGR_FLUSH, LOGMGR_PMM_LOG, chkpt, Write telog|
|**15**|**Netzwerk-IO**|ASYNC_NETWORK_IO, NET_WAITFOR_PACKET PROXY_NETWORK_IO, EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**Parallelität**|cxpacket, Exchange, HT%, BMP%, BP%|
|**17**|**Arbeitsspeicher**|RESOURCE_SEMAPHORE, cmemthread, cmempartitioniert, EE_PMOLOCK, MEMORY_ALLOCATION_EXT, RESERVED_MEMORY_ALLOCATION_EXT, MEMORY_GRANT_UPDATE|
|**Jahren**|**Benutzer Wartezeit**|WAITFOR, WAIT_FOR_RESULTS BROKER_RECEIVE_WAITFOR|
|**19**|**Ablaufverfolgung**|tracewrite, SQLTRACE_LOCK, SQLTRACE_FILE_BUFFER, SQLTRACE_FILE_WRITE_IO_COMPLETION, SQLTRACE_FILE_READ_IO_COMPLETION, SQLTRACE_PENDING_BUFFER_WRITERS, SQLTRACE_SHUTDOWN, QUERY_TRACEOUT|
|**20**|**Voll Text Suche**|FT_RESTART_CRAWL, Volltext-Gatherer, MSSearch, FT_METADATA_MUTEX, FT_IFTSHC_MUTEX, FT_IFTSISM_MUTEX, FT_IFTS_RWLOCK, FT_COMPROWSET_RWLOCK, FT_MASTER_MERGE, FT_PROPERTYLIST_CACHE, FT_MASTER_MERGE_COORDINATOR|
|**21**|**Sonstige e/a**|ASYNC_IO_COMPLETION, IO_COMPLETION, backupio, WRITE_COMPLETION, IO_QUEUE_LIMIT, IO_RETRY|
|**22**|**Replikation**|SE_REPL_%, REPL_%, HADR_% **(aber nicht HADR_THROTTLE_LOG_RATE_GOVERNOR)**, PWAIT_HADR_%, REPLICA_WRITES, FCB_REPLICA_WRITE, FCB_REPLICA_READ, PWAIT_HADRSIM|
|**23**|**Protokoll Raten Kontrolle**|LOG_RATE_GOVERNOR, POOL_LOG_RATE_GOVERNOR HADR_THROTTLE_LOG_RATE_GOVERNOR, INSTANCE_LOG_RATE_GOVERNOR|

Die **Kompilierungs** Wartezeit wird derzeit nicht unterstützt.

## <a name="permissions"></a>Berechtigungen

 Erfordert die `VIEW DATABASE STATE`-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen

- [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
- [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
- [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
- [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
- [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
- [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
- [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
