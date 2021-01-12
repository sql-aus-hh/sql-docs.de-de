---
description: sys.dm_resource_governor_resource_pools (Transact-SQL)
title: sys.dm_resource_governor_resource_pools (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pools_TSQL
- dm_resource_governor_resource_pools_TSQL
- sys.dm_resource_governor_resource_pools
- dm_resource_governor_resource_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_resource_pools dynamic management view
ms.assetid: 9bfc926e-d8bc-40f8-9229-ab1f8a1e69c5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ac359e1b73d963e7c2d7f880be7443118f8bd2c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101456"
---
# <a name="sysdm_resource_governor_resource_pools-transact-sql"></a>sys.dm_resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt Informationen zum aktuellen Status der Ressourcenpools, zur aktuellen Konfiguration der Ressourcenpools sowie Statistiken zu den Ressourcenpools zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_resource_governor_resource_pools**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|Die ID des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|name|**sysname**|Der Name des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|statistics_start_time|**datetime**|Der Zeitpunkt, zu dem Statistiken für diesen Pool zurückgesetzt wurden. Lässt keine NULL-Werte zu.|  
|total_cpu_usage_ms|**bigint**|Die kumulierte CPU-Verwendung in Millisekunden, seitdem die Ressourcenkontrollstatistiken zurückgesetzt wurden. Lässt keine NULL-Werte zu.|  
|cache_memory_kb|**bigint**|Die gesamte aktuelle Cachespeicherverwendung in Kilobyte. Lässt keine NULL-Werte zu.|  
|compile_memory_kb|**bigint**|Die aktuell verwendete (gestohlene) Arbeitsspeicher in Kilobyte (KB). Der Arbeitsspeicher wird hierbei hauptsächlich für die Kompilierung und Optimierung verwendet, kann jedoch auch zu anderen Zwecken verwendet werden. Lässt keine NULL-Werte zu.|  
|used_memgrant_kb|**bigint**|Der gesamte aktuell verwendete (gestohlene) Arbeitsspeicher aus der Arbeitsspeicherzuweisung. Lässt keine NULL-Werte zu.|  
|total_memgrant_count|**bigint**|Die kumulierte Arbeitsspeicherzuweisung in diesem Ressourcenpool. Lässt keine NULL-Werte zu.|  
|total_memgrant_timeout_count|**bigint**|Die kumulierten Arbeitsspeicherzuweisungs-Timeouts in diesem Ressourcenpool. Lässt keine NULL-Werte zu.|  
|active_memgrant_count|**int**|Die aktuelle Anzahl von Arbeitsspeicherzuweisungen. Lässt keine NULL-Werte zu.|  
|active_memgrant_kb|**bigint**|Die Summe der aktuellen Arbeitsspeicherzuweisungen in Kilobyte (KB). Lässt keine NULL-Werte zu.|  
|memgrant_waiter_count|**int**|Die Anzahl von zurzeit ausstehenden Abfragen für Arbeitsspeicherzuweisungen. Lässt keine NULL-Werte zu.|  
|max_memory_kb|**bigint**|Die maximale Arbeitsspeichermenge in Kilobyte, über die der Ressourcenpool verfügen kann. Dies basiert auf den aktuellen Einstellungen und dem Serverstatus. Lässt keine NULL-Werte zu.|  
|used_memory_kb|**bigint**|Der Arbeitsspeicher in Kilobyte, der für den Ressourcenpool verwendet wird. Lässt keine NULL-Werte zu.|  
|target_memory_kb|**bigint**|Die Zielmenge an Arbeitsspeicher in Kilobyte, die der Ressourcenpool zu erlangen versucht. Dies basiert auf den aktuellen Einstellungen und dem Serverstatus. Lässt keine NULL-Werte zu.|  
|out_of_memory_count|**bigint**|Die Anzahl der Speicherbelegungsfehler im Pool, seitdem die Ressourcenkontrollstatistiken zurückgesetzt wurden. Lässt keine NULL-Werte zu.|  
|min_cpu_percent|**int**|Die aktuelle Konfiguration für die garantierte durchschnittliche CPU-Bandbreite für alle Anforderungen im Ressourcenpool, wenn CPU-Konflikte bestehen. Lässt keine NULL-Werte zu.|  
|max_cpu_percent|**int**|Die aktuelle Konfiguration für die maximale durchschnittliche CPU-Bandbreite, die für alle Anforderungen im Ressourcenpool zulässig ist, wenn CPU-Konflikte bestehen. Lässt keine NULL-Werte zu.|  
|min_memory_percent|**int**|Die aktuelle Konfiguration für die garantierte Arbeitsspeichermenge für alle Anforderungen im Ressourcenpool, wenn Arbeitsspeicherkonflikte bestehen. Dieser Arbeitsspeicher wird nicht mit anderen Ressourcenpools gemeinsam genutzt. Lässt keine NULL-Werte zu.|  
|max_memory_percent|**int**|Die aktuelle Konfiguration des Prozentsatzes des gesamten Serverspeichers, der für Anforderungen in diesem Ressourcenpool verwendet werden kann. Lässt keine NULL-Werte zu.|  
|cap_cpu_percent|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Feste Obergrenze der CPU-Bandbreite, die allen Anforderungen im Ressourcenpool zugewiesen wird. Beschränkt die maximale CPU-Bandbreitenstufe auf die angegebene Stufe. Der zulässige Bereich für den Wert ist 1 bis 100. Lässt keine NULL-Werte zu.|  
|min_iops_per_volume|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die minimalen E/A-Vorgänge pro Sekunde (IOPS) pro Volumeeinstellung für diesen Pool. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|max_iops_per_volume|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Einstellung für die maximale e/a pro Sekunde (IOPS) pro Datenträger Volume für diesen Pool. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, die Einstellungen für MIN_IOPS_PER_VOLUME und MAX_IOPS_PER_VOLUME des Ressourcenpools sind 0.|  
|read_io_queued_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrolle in die Warteschlange eingereiht wurden. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|read_io_issued_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken ausgegeben wurden. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|read_io_completed_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken abgeschlossen wurden. Lässt keine NULL-Werte zu.|  
|read_io_throttled_total|**int**|Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken gedrosselt wurden. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|read_bytes_total|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl von Bytes, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken gelesen wurden. Lässt keine NULL-Werte zu.|  
|read_io_stall_total_ms|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtzeit (in Millisekunden) zwischen dem Eingang und Abschluss von E/A-Lesevorgängen. Lässt keine NULL-Werte zu. |  
|read_io_stall_queued_ms|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Gesamtzeit (in Millisekunden) zwischen Lese-e/a-Empfang und Problem. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.<br /><br /> Um zu ermitteln, ob die e/a-Einstellung für den Pool Latenz verursacht, subtrahieren Sie **read_io_stall_queued_ms** von **read_io_stall_total_ms**.|  
|write_io_queued_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken in die Warteschlange eingereiht wurden. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|write_io_issued_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken ausgegeben wurden. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|write_io_completed_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken abgeschlossen wurden. Lässt keine NULL-Werte zu.|  
|write_io_throttled_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken gedrosselt wurden. Lässt keine NULL-Werte zu.|  
|write_bytes_total|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl von Bytes, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken geschrieben wurden. Lässt keine NULL-Werte zu.|  
|write_io_stall_total_ms|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Gesamtzeit (in Millisekunden) zwischen Schreib-e/a-Eingang und-Abschluss. Lässt keine NULL-Werte zu. |  
|write_io_stall_queued_ms|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtzeit (in Millisekunden) zwischen dem Eingang und Abschluss von E/A-Schreibvorgängen. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.<br /><br /> Dies ist die Verzögerung, die durch die Ressourcenkontrolle für E/A-Vorgänge eingeführt wird.|  
|io_issue_violations_total|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtanzahl der E/A-Ausgabeverletzungen. Das heißt, wie häufig die E/A-Ausgaberate unter der reservierten Rate lag. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|io_issue_delay_total_ms|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die Gesamtzeit (in Millisekunden) zwischen der geplanten Ausgabe und tatsächlichen Ausgabe von E/A-Vorgängen. Lässt NULL-Werte zu. NULL, wenn E/A-Vorgänge für den Ressourcenpool nicht kontrolliert werden. Das heißt, der Ressourcen Pool MIN_IOPS_PER_VOLUME und die MAX_IOPS_PER_VOLUME Einstellungen sind 0.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="remarks"></a>Bemerkungen  
 Arbeitsauslastungsgruppen und Ressourcenpools der Ressourcenkontrolle weisen eine n:1-Zuordnung auf. Daher werden viele Ressourcenpoolstatistiken von Arbeitsauslastungsstatistiken abgeleitet.  
  
 Diese dynamische Verwaltungssicht zeigt die Konfiguration im Arbeitsspeicher an. Verwenden Sie die sys.resource_governor_resource_pools-Katalog Sicht, um die gespeicherten Konfigurations Metadaten anzuzeigen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  



