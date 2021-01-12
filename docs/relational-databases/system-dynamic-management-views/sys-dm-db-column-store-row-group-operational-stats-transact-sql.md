---
description: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8ff8ab3db826d483f8f6df09277e1a5510aaf48
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097710"
---
# <a name="sysdm_db_column_store_row_group_operational_stats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Gibt aktuelle e/a-, Sperr-und Zugriffsmethoden Aktivitäten auf Zeilenebene für komprimierte Zeilen Gruppen in einem columnstore--Index zurück. Verwenden Sie **sys.dm_db_column_store_row_group_operational_stats** , um zu überprüfen, wie lange eine Benutzer Abfrage auf das Lesen oder schreiben in eine komprimierte Zeilen Gruppe oder Partition eines columnstore--Indexes warten muss, und identifiziert Zeilen Gruppen, die auf eine signifikante e/a-Aktivität oder an Hotspots stoßen.  
  
 In-Memory-columnstore--Indizes werden in dieser DMV nicht angezeigt.  
 
 
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Die ID der Tabelle mit dem columnstore--Index.|  
|**index_id**|**int**|ID des columnstore-Indexes.|  
|**partition_number**|**int**|Auf 1 basierende Partitionsnummer im Index oder Heap.|  
|**row_group_id**|**int**|ID der Zeilen Gruppe im columnstore--Index. Dies ist innerhalb einer Partition eindeutig.|  
|**scan_count**|**int**|Anzahl der Scans durch die Zeilen Gruppe seit dem letzten SQL-Neustart.|  
|**delete_buffer_scan_count**|**int**|Gibt an, wie oft der Lösch Puffer zum Ermitteln gelöschter Zeilen in dieser Zeilen Gruppe verwendet wurde. Dies schließt den Zugriff auf die in-Memory-Hash Tabelle und die zugrunde liegende BTREE ein.|  
|**index_scan_count**|**int**|Gibt an, wie oft die columnstore--Index Partition gescannt wurde. Dies ist für alle Zeilen Gruppen in der Partition identisch.|  
|**rowgroup_lock_count**|**bigint**|Kumulierte Anzahl der Sperr Anforderungen für diese Zeilen Gruppe seit dem letzten SQL-Neustart.|  
|**rowgroup_lock_wait_count**|**bigint**|Kumulierte Häufigkeit, mit der die Datenbank-Engine seit dem letzten SQL-Neustart auf diese Zeilen Gruppen Sperre gewartet hat.|  
|**rowgroup_lock_wait_in_ms**|**bigint**|Kumulierte Anzahl der Millisekunden, die die Datenbank-Engine seit dem letzten SQL-Neustart auf diese Zeilen Gruppen Sperre gewartet hat.|  
  
## <a name="permissions"></a>Berechtigungen  
 Folgende Berechtigungen sind erforderlich:  
  
-   Control-Berechtigung für die durch object_id angegebene Tabelle.  
  
-   VIEW DATABASE STATE-Berechtigung zum Zurückgeben von Informationen zu allen Objekten innerhalb der Datenbank, mithilfe des Objekt Platzhalters @*object_id* = NULL  
  
 Wenn die VIEW DATABASE STATE-Berechtigung erteilt wurde, ist die Rückgabe für alle Objekte in der Datenbank zulässig, unabhängig davon, ob CONTROL-Berechtigungen für bestimmte Objekte verweigert wurden.  
  
 Nach dem Verweigern der VIEW DATABASE STATE-Berechtigung können keine Objekte in der Datenbank zurückgegeben werden, unabhängig von möglicherweise erteilten CONTROL-Berechtigungen für bestimmte Objekte. Wenn der Datenbank-Platzhalter @*database_id*= NULL angegeben wird, wird die Datenbank auch weggelassen.  
  
 Weitere Informationen finden Sie unter [dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Index Related Dynamic Management Views and Functions (Transact-SQL) (Indexbezogene dynamische Verwaltungssichten und -funktionen (Transact-SQL))](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  

