---
description: sys.dm_resource_governor_resource_pool_volumes (Transact-SQL)
title: sys.dm_resource_governor_resource_pool_volumes (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pool_volumes_TSQL
- dm_resource_governor_resource_pool_volumes_TSQL
- dm_resource_governor_resource_pool_volumes
- sys.dm_resource_governor_resource_pool_volumes
dev_langs:
- TSQL
helpviewer_keywords:
- dm_resource_governor_resource_pool_volumes
- sys.dm_resource_governor_resource_pool_volumes
ms.assetid: fa692e56-c561-4533-97c5-bc12c600553f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9c6707bfd8b1c29073239821cd0fa3c0425ef089
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093984"
---
# <a name="sysdm_resource_governor_resource_pool_volumes-transact-sql"></a>sys.dm_resource_governor_resource_pool_volumes (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu den aktuellen Ressourcenpool-E/A-Statistiken für jeden Datenträger zurück. Diese Informationen sind auch auf Ressourcenpool Ebene in [sys.dm_resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)verfügbar.  
  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|Die ID des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|volume_name|**sysname**|Der Name des Datenträgervolumes. Lässt keine NULL-Werte zu.|  
|read_io_queued_total|**int**|Die Gesamtanzahl der gelesenen e/a-Vorgänge, die seit dem Zurücksetzen des Ressourcen-goven Lässt keine NULL-Werte zu.|  
|read_io_issued_total|**int**|Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken ausgegeben wurden. Lässt keine NULL-Werte zu.|  
|read_ios_completed_total|**int**|Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken abgeschlossen wurden. Lässt keine NULL-Werte zu.|  
|read_ios_throttled_total|**int**|Die Gesamtanzahl der E/A-Lesevorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken gedrosselt wurden. Lässt keine NULL-Werte zu.|  
|read_bytes_total|**bigint**|Die Gesamtanzahl von Bytes, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken gelesen wurden. Lässt keine NULL-Werte zu.|  
|read_io_stall_total_ms|**bigint**|Die Gesamtzeit (in Millisekunden) zwischen dem Eingang und Abschluss von E/A-Lesevorgängen. Lässt keine NULL-Werte zu.|  
|read_io_stall_queued_ms|**bigint**|Gesamtzeit (in Millisekunden) zwischen Lese-e/a-Empfang und Problem. Dies ist die Verzögerung, die durch die Ressourcenkontrolle für E/A-Vorgänge eingeführt wird. Lässt keine NULL-Werte zu.|  
|write_io_queued_total|**int**|Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken in die Warteschlange eingereiht wurden. Lässt keine NULL-Werte zu.|  
|write_io_issued_total|**int**|Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken ausgegeben wurden. Lässt keine NULL-Werte zu.|  
|write_io_completed_total|**int**|Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken abgeschlossen wurden. Lässt keine NULL-Werte zu.|  
|write_io_throttled_total|**int**|Die Gesamtanzahl der E/A-Schreibvorgänge, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken gedrosselt wurden. Lässt keine NULL-Werte zu.|  
|write_bytes_total|**bigint**|Die Gesamtanzahl von Bytes, die seit dem Zurücksetzen der Ressourcenkontrollstatistiken geschrieben wurden. Lässt keine NULL-Werte zu.|  
|write_io_stall_total_ms|**bigint**|Die Gesamtzeit (in Millisekunden) zwischen der Ausgabe und dem Abschluss von E/A-Schreibvorgängen. Lässt keine NULL-Werte zu.|  
|write_io_stall_queued_ms|**bigint**|Die Gesamtzeit (in Millisekunden) zwischen dem Eingang und Abschluss von E/A-Schreibvorgängen. Dies ist die Verzögerung, die durch die Ressourcenkontrolle für E/A-Vorgänge eingeführt wird. Lässt keine NULL-Werte zu.|  
|io_issue_violations_total|**int**|Die Gesamtanzahl der E/A-Ausgabeverletzungen. Das heißt, wie häufig die E/A-Ausgaberate unter der reservierten Rate lag. Lässt keine NULL-Werte zu.|  
|io_issue_delay_total_ms|**bigint**|Die Gesamtzeit (in Millisekunden) zwischen der geplanten Ausgabe und tatsächlichen Ausgabe von E/A-Vorgängen. Lässt keine NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  

