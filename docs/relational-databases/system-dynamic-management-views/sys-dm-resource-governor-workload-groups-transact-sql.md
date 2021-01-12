---
description: sys.dm_resource_governor_workload_groups (Transact-SQL)
title: sys.dm_resource_governor_workload_groups (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_workload_groups
- sys.dm_resource_governor_workload_groups_TSQL
- dm_resource_governor_workload_groups
- dm_resource_governor_workload_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_workload_groups dynamic management view
ms.assetid: f63c4914-1272-43ef-b135-fe1aabd953e0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b38844c891edaf9a7592a2dd573c86cc5fa1fcb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097546"
---
# <a name="sysdm_resource_governor_workload_groups-transact-sql"></a>sys.dm_resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt Statistiken zu Arbeitsauslastungsgruppen sowie die aktuelle Konfiguration der Arbeitsauslastungsgruppen im Arbeitsspeicher zurück. Diese Sicht kann mit sys.dm_resource_governor_resource_pools verknüpft werden, um den Ressourcenpoolnamen abzurufen.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_resource_governor_workload_groups**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|group_id|**int**|ID der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|  
|name|**sysname**|Name der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|  
|pool_id|**int**|ID des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|external_pool_id|**int**|**Gilt für**: ab [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] .<br /><br /> ID des externen Ressourcenpools. Lässt keine NULL-Werte zu.|  
|statistics_start_time|**datetime**|Uhrzeit, zu der die Statistikauflistung für die Arbeitsauslastungsgruppe zurückgesetzt wurde. Lässt keine NULL-Werte zu.|  
|total_request_count|**bigint**|Kumulierte Anzahl vervollständigter Anforderungen in der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|  
|total_queued_request_count|**bigint**|Kumulierte Anzahl von Anforderungen, die in die Warteschlange gestellt wurden, nachdem die GROUP_MAX_REQUESTS-Grenze erreicht wurde. Lässt keine NULL-Werte zu.|  
|active_request_count|**int**|Die aktuelle Anforderungsanzahl. Lässt keine NULL-Werte zu.|  
|queued_request_count|**int**|Die Anzahl der zurzeit in der Warteschlange befindlichen Anforderungen. Lässt keine NULL-Werte zu.|  
|total_cpu_limit_violation_count|**bigint**|Kumulierte Anzahl von Anforderungen, die die CPU-Grenze übersteigen. Lässt keine NULL-Werte zu.|  
|total_cpu_usage_ms|**bigint**|Kumulierte CPU-Verwendung dieser Arbeitsauslastungsgruppe in Millisekunden. Lässt keine NULL-Werte zu.|  
|max_request_cpu_time_ms|**bigint**|Maximale CPU-Nutzung für eine einzelne Anforderung in Millisekunden. Lässt keine NULL-Werte zu.<br /><br /> **Hinweis:** Hierbei handelt es sich um einen gemessenen Wert, im Gegensatz zu REQUEST_MAX_CPU_TIME_SEC, eine konfigurierbare Einstellung. Weitere Informationen finden Sie unter [CPU Threshold Exceeded (Ereignisklasse)](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|blocked_task_count|**int**|Aktuelle Anzahl blockierter Tasks. Lässt keine NULL-Werte zu.|  
|total_lock_wait_count|**bigint**|Kumulierte Anzahl von Sperrwartezeiten, die aufgetreten sind. Lässt keine NULL-Werte zu.|  
|total_lock_wait_time_ms|**bigint**|Kumulierte Summe der verstrichenen Zeit einer Sperre in Millisekunden. Lässt keine NULL-Werte zu.|  
|total_query_optimization_count|**bigint**|Kumulierte Anzahl von Abfrageoptimierungen in dieser Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|  
|total_suboptimal_plan_generation_count|**bigint**|Kumulierte Anzahl von nicht optimalen Planerstellungen, die aufgrund des nicht ausreichenden Arbeitsspeichers in dieser Arbeitsauslastungsgruppe aufgetreten sind. Lässt keine NULL-Werte zu.|  
|total_reduced_memgrant_count|**bigint**|Kumulierte Anzahl von Arbeitsspeicherzuweisungen, die die maximale Abfragegrößenbeschränkung erreicht haben. Lässt keine NULL-Werte zu.|  
|max_request_grant_memory_kb|**bigint**|Maximale Arbeitsspeicherzuweisungsgröße einer einzelnen Anforderung, seit die Statistik zurückgesetzt wurde, in Kilobyte. Lässt keine NULL-Werte zu.|  
|active_parallel_thread_count|**bigint**|Aktuelle Anzahl der parallelen Thread Verwendung. Lässt keine NULL-Werte zu.|  
|importance|**sysname**|Aktueller Konfigurationswert für die relative Wichtigkeit einer Anforderung in dieser Arbeitsauslastungsgruppe. Die Wichtigkeit ist eine der folgenden Werte, wobei Medium die Standardeinstellung ist: niedrig, Mittel oder hoch.<br /><br /> Lässt keine NULL-Werte zu.|  
|request_max_memory_grant_percent|**int**|Aktuelle Einstellung der maximalen Arbeitsspeicherzuweisung in Prozent für eine einzelne Anforderung. Lässt keine NULL-Werte zu.|  
|request_max_cpu_time_sec|**int**|Aktuelle Einstellung für den maximalen CPU-Nutzungsgrenzwert für eine einzelne Anforderung in Sekunden. Lässt keine NULL-Werte zu.|  
|request_memory_grant_timeout_sec|**int**|Aktuelle Einstellung für das Timeout der Arbeitsspeicherzuweisung für eine einzelne Anforderung in Sekunden. Lässt keine NULL-Werte zu.|  
|group_max_requests|**int**|Aktuelle Einstellung für die maximale Anzahl gleichzeitiger Anforderungen. Lässt keine NULL-Werte zu.|  
|max_dop|**int**|Der maximale Grad an Parallelität für die Arbeits Auslastungs Gruppe wurde konfiguriert. Der Standardwert 0 verwendet globale Einstellungen. Lässt keine NULL-Werte zu.| 
|effective_max_dop|**int**|**Gilt für**: ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .<br /><br />Effektiver maximaler Grad an Parallelität für die Arbeits Auslastungs Gruppe. Lässt keine NULL-Werte zu.| 
|total_cpu_usage_preemptive_ms|**bigint**|**Gilt für**: ab [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] .<br /><br />Gesamte CPU-Zeit, die bei der Planung des präemptiven Modus für die Arbeits Auslastungs Gruppe in MS gemessen wird. Lässt keine NULL-Werte zu.<br /><br />Für die Ausführung von Code außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (z. B. erweiterte gespeicherte Prozeduren und verteilte Abfragen) muss ein Thread außerhalb der Steuerung des nicht präemptiven Zeitplanungsmoduls ausgeführt werden. Dazu wechselt ein Arbeitsthread in den präemptiven Modus.| 
|request_max_memory_grant_percent_numeric|**float**|**Gilt für**: ab [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] .<br /><br />Aktuelle Einstellung der maximalen Arbeitsspeicherzuweisung in Prozent für eine einzelne Anforderung. Lässt keine NULL-Werte zu.| 
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="remarks"></a>Bemerkungen  
 Diese dynamische Verwaltungssicht zeigt die Konfiguration im Arbeitsspeicher an. Verwenden Sie die [sys.resource_governor_workload_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md) -Katalog Sicht, um die gespeicherten Konfigurations Metadaten anzuzeigen.  
  
 Wenn `ALTER RESOURCE GOVERNOR RESET STATISTICS` erfolgreich ausgeführt wurde, werden die folgenden Zähler zurückgesetzt: `statistics_start_time` , `total_request_count` , `total_queued_request_count` , `total_cpu_limit_violation_count` , `total_cpu_usage_ms` , `max_request_cpu_time_ms` , `total_lock_wait_count` , `total_lock_wait_time_ms` , `total_query_optimization_count` , `total_suboptimal_plan_generation_count` , `total_reduced_memgrant_count` und `max_request_grant_memory_kb` . Der Zähler `statistics_start_time` wird auf das aktuelle Systemdatum und die aktuelle System Uhrzeit festgelegt, und die anderen Indikatoren werden auf 0 (null) festgelegt.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die `VIEW SERVER STATE`-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [sys.resource_governor_workload_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
