---
description: sys.dm_db_xtp_index_stats (Transact-SQL)
title: sys.dm_db_xtp_index_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_index_stats
- dm_db_xtp_index_stats
- sys.dm_db_xtp_index_stats_TSQL
- dm_db_xtp_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_index_stats dynamic management view
ms.assetid: 8d0a50b8-2015-4576-930f-e3307dfc888e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0916891a629298602c274d4cf787e6bd9e1a8a9f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099911"
---
# <a name="sysdm_db_xtp_index_stats-transact-sql"></a>sys.dm_db_xtp_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Enthält Statistiken, die seit dem letzten Neustart der Datenbank erfasst wurden.  
  
 Weitere Informationen finden Sie unter [in-Memory OLTP &#40;In-Memory Optimization&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md) und [Richtlinien für die Verwendung von Indizes in Memory-Optimized Tabellen](/previous-versions/sql/sql-server-2016/dn133166(v=sql.130)).  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|ID des Objekts, zu dem dieser Index gehört.|  
|xtp_object_id|**bigint**|Interne ID, die der aktuellen Version des-Objekts entspricht.<br /><br /> Hinweis: gilt für [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] .|  
|index_id|**bigint**|Die ID des Index. Die index_id ist nur innerhalb des Objekts eindeutig.|  
|scans_started|**bigint**|Anzahl der ausgeführten In-Memory OLTP-Indexscans. Jeder Auswähl-, Einfüge-, Update- oder Löschvorgang erfordert einen Indexscan.|  
|scans_retries|**bigint**|Anzahl der Indexscans, die wiederholt werden mussten.|  
|rows_returned|**bigint**|Kumulierte Anzahl der Zeilen, die seit dem Erstellen der Tabelle oder dem Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurückgegeben wurden.|  
|rows_touched|**bigint**|Kumulierte Anzahl der Zeilen, auf die seit dem Erstellen der Tabelle oder dem Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugegriffen wurde.|  
|rows_expiring|**bigint**|Nur zur internen Verwendung.|  
|rows_expired|**bigint**|Nur zur internen Verwendung.|  
|rows_expired_removed|**bigint**|Nur zur internen Verwendung.|  
|phantom_scans_started|**bigint**|Nur zur internen Verwendung.|  
|phantom_scans_retries|**bigint**|Nur zur internen Verwendung.|  
|phantom_rows_touched|**bigint**|Nur zur internen Verwendung.|  
|phantom_expiring_rows_encountered|**bigint**|Nur zur internen Verwendung.|  
|phantom_expired_rows_encountered|**bigint**|Nur zur internen Verwendung.|  
|phantom_expired_removed_rows_encountered|**bigint**|Nur zur internen Verwendung.|  
|phantom_expired_rows_removed|**bigint**|Nur zur internen Verwendung.|  
|object_address|**varbinary(8)**|Nur zur internen Verwendung.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung für die aktuelle Datenbank.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
