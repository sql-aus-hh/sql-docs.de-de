---
description: sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
title: sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_nonclustered_index_stats_TSQL
- dm_db_xtp_nonclustered_index_stats
- sys.dm_db_xtp_nonclustered_index_stats_TSQL
- sys.dm_db_xtp_nonclustered_index_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_nonclustered_index_stats
ms.assetid: d55ba31c-296c-419b-9c4b-c126e0a3d156
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1c093479ffad2ea852d9af0806f0e78ac907773a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099878"
---
# <a name="sysdm_db_xtp_nonclustered_index_stats-transact-sql"></a>sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  "sys.dm_db_xtp_nonclustered_index_stats" enthält Statistikdaten zu Vorgängen, die für nicht gruppierte Indizes in speicheroptimierten Tabellen ausgeführt werden. "sys.dm_db_xtp_nonclustered_index_stats" enthält eine Zeile für jeden nicht gruppierten Index einer speicheroptimierten Tabelle in der aktuellen Datenbank.  
  
 Die in "sys.dm_db_xtp_nonclustered_index_stats" enthaltenen Statistikdaten werden bei der Erstellung der In-Memory-Indexstruktur erfasst. In-Memory-Indexstrukturen werden beim Neustart der Datenbank neu erstellt.  
  
 Verwenden Sie sys.dm_db_xtp_nonclustered_index_stats, um die Indexaktivitäten zu verstehen und zu überwachen, während DML-Vorgänge ausgeführt werden und eine Datenbank online geschaltet wird. Wenn eine Datenbank mit einer speicheroptimierten Tabelle neu gestartet wird, wird der Index erstellt, indem jeweils eine Zeile in den Arbeitsspeicher eingefügt wird. Anhand der Anzahl der Seitenteilungen, Zusammenführungen und Konsolidierungen können Sie nachvollziehen, welche Schritte zur Indexerstellung ausgeführt werden, wenn eine Datenbank online geschaltet wird. Sie können diese Werte auch vor und nach einer Reihe von DML-Vorgängen vergleichen.  
  
 Eine große Anzahl wiederholter Versuche weist auf Parallelitätsprobleme hin; wenden Sie sich an den [!INCLUDE[msCoName](../../includes/msconame-md.md)] Support.  
  
 Weitere Informationen zu Speicher optimierten, nicht gruppierten Indizes finden Sie unter [SQL Server In-Memory OLTP Internals Übersicht](https://t.co/T6zToWc6y6), Seite 17.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID des Objekts.|  
|xtp_object_id|**bigint**|Die ID der Speicher optimierten Tabelle.|  
|index_id|**int**|Die ID des Index.|  
|delta_pages|**bigint**|Die Gesamtanzahl der Änderungsseiten für diesen Index in der Struktur.|  
|internal_pages|**bigint**|Für die interne Verwendung. Die Gesamtanzahl der internen Seiten für diesen Index in der Struktur.|  
|leaf_pages|**bigint**|Die Gesamtanzahl der Blattseiten für diesen Index in der Struktur.|  
|outstanding_retired_nodes|**bigint**|Für die interne Verwendung. Die Gesamtanzahl der Knoten für diesen Index in den internen Strukturen.|  
|page_update_count|**bigint**|Die kumulative Anzahl der Updatevorgänge für eine Seite im Index.|  
|page_update_retry_count|**bigint**|Die kumulative Anzahl der wiederholten Updatevorgänge für eine Seite im Index.|  
|page_consolidation_count|**bigint**|Die kumulative Anzahl der Seitenkonsolidierungen im Index.|  
|page_consolidation_retry_count|**bigint**|Die kumulative Anzahl der wiederholten Seitenkonsolidierungen.|  
|page_split_count|**bigint**|Die kumulative Anzahl der Seitenteilungsvorgänge im Index.|  
|page_split_retry_count|**bigint**|Die kumulative Anzahl der wiederholten Seitenteilungsvorgänge.|  
|key_split_count|**bigint**|Die kumulative Anzahl der Schlüsselteilungen im Index.|  
|key_split_retry_count|**bigint**|Die kumulative Anzahl der wiederholten Schlüsselteilungsvorgänge.|  
|page_merge_count|**bigint**|Die kumulative Anzahl der Seitenzusammenführungen im Index.|  
|page_merge_retry_count|**bigint**|Die kumulative Anzahl der wiederholten Seitenzusammenführungen.|  
|key_merge_count|**bigint**|Die kumulative Anzahl der Schlüsselzusammenführungen im Index.|  
|key_merge_retry_count|**bigint**|Die kumulative Anzahl der wiederholten Schlüsselzusammenführungen.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung für die aktuelle Datenbank.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
