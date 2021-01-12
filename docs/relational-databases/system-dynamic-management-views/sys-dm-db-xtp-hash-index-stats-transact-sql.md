---
description: sys.dm_db_xtp_hash_index_stats (Transact-SQL)
title: sys.dm_db_xtp_hash_index_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_hash_index_stats
- sys.dm_db_xtp_hash_index_stats_TSQL
- dm_db_xtp_hash_index_stats
- dm_db_xtp_hash_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_hash_index_stats (dynamic management view)
ms.assetid: 45969884-cd61-48e8-aee5-c725c78e3e4c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 239ca2e5a60f8ffeeafd807316e5a903dc840754
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092879"
---
# <a name="sysdm_db_xtp_hash_index_stats-transact-sql"></a>sys.dm_db_xtp_hash_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Diese Statistik ist für das Verständnis und die Optimierung der Bucketanzahl hilfreich. Sie kann auch verwendet werden, um Fälle zu erkennen, in denen der Indexschlüssel viele Duplikate aufweist.  
  
 Eine große durchschnittliche Kettenlänge weist darauf hin, dass viele Zeilen demselben Hashbucket hinzugefügt werden. Dies könnte folgende Ursachen haben:  
  
-   Wenn die Anzahl der leeren Buckets niedrig ist oder die durchschnittliche und die maximale Kettenlänge ähnlich sind, besteht die Wahrscheinlichkeit, dass die Bucketgesamtanzahl zu niedrig ist. Dies führt dazu, dass viele verschiedene Indexschlüssel demselben Hashbucket hinzugefügt werden.  
  
-   Wenn die Anzahl der leeren Buckets hoch oder die maximale Kettenlänge relativ zur durchschnittlichen Kettenlänge hoch ist, besteht die Wahrscheinlichkeit, dass viele Zeilen mit doppelten Indexschlüsselwerten vorliegen oder dass die Schlüsselwerte verzerrt sind. Alle Zeilen mit demselben Indexschlüsselwert werden demselben Hashbucket hinzugefügt. Daher weist dieser Bucket eine große Kettenlänge auf.  
  
Hohe Kettenlängen können die Leistung aller DML-Vorgänge für einzelne Zeilen, einschließlich von SELECT oder INSERT, deutlich beeinträchtigen. Kurze Kettenlängen in Kombination mit einer hohen Anzahl von leeren Buckets weisen auf eine zu hohe Bucketanzahl hin. Dadurch wird die Leistung von Indexscans verringert.  
  
> [!WARNING]
> **sys.dm_db_xtp_hash_index_stats** scannt die gesamte Tabelle. Wenn also große Tabellen in der Datenbank vorhanden sind, kann **sys.dm_db_xtp_hash_index_stats** viel Zeit in Anspruch nehmen.  
  
Weitere Informationen finden Sie unter [Hash Indizes für Memory-Optimized Tabellen](../../relational-databases/sql-server-index-design-guide.md#hash_index).  
  
|Spaltenname|type|Beschreibung|  
|-----------------|----------|-----------------|  
|object_id|**int**|Die Objekt-ID der übergeordneten Tabelle.|  
|xtp_object_id|**bigint**|Die ID der Speicher optimierten Tabelle.|  
|index_id|**int**|Die Index-ID.|  
|total_bucket_count|**bigint**|Die Gesamtanzahl der Hashbuckets im Index.|  
|empty_bucket_count|**bigint**|Die Anzahl der leeren Hashbuckets im Index.|  
|avg_chain_length|**bigint**|Die durchschnittliche Länge der Zeilenketten für alle Hashbuckets im Index.|  
|max_chain_length|**bigint**|Die maximale Länge der Zeilenketten in den Hashbuckets.|  
|xtp_object_id|**bigint**|Die in-Memory-OLTP-Objekt-ID, die der Speicher optimierten Tabelle entspricht.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung auf dem Server.  

## <a name="examples"></a>Beispiele  
  
### <a name="a-troubleshooting-hash-index-bucket-count"></a>A. Problembehandlung bei Hashindex-Bucketanzahl

Die folgende Abfrage kann verwendet werden, um eine Problembehandlung für die Bucketanzahl der Hashindex einer vorhandenen Tabelle durchzusetzen. Die Abfrage gibt Statistiken zum Prozentsatz leerer Bucket und Kettenlänge für alle Hash Indizes in Benutzer Tabellen zurück.

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [table],   
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    FLOOR((  
      CAST(h.empty_bucket_count as float) /  
        h.total_bucket_count) * 100)  
                             as [empty_bucket_percent],  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type=1
  ORDER BY [table], [index];  
``` 

Weitere Informationen zum Interpretieren der Ergebnisse dieser Abfrage finden Sie unter Problembehandlung [bei Hash Indizes für Memory-Optimized Tabellen](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md) .  

### <a name="b-hash-index-statistics-for-internal-tables"></a>B. Hash Index Statistik für interne Tabellen

Bestimmte Features verwenden interne Tabellen, die Hash Indizes nutzen, z. b. columnstore--Indizes für Speicher optimierte Tabellen. Die folgende Abfrage gibt Statistiken für Hash Indizes für interne Tabellen zurück, die mit Benutzer Tabellen verknüpft sind.

```sql
  SELECT  
    QUOTENAME(SCHEMA_NAME(t.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(h.object_id)) as [user_table],
    ia.type_desc as [internal_table_type],
    i.name                   as [index],   
    h.total_bucket_count,  
    h.empty_bucket_count,  
    h.avg_chain_length,   
    h.max_chain_length  
  FROM sys.dm_db_xtp_hash_index_stats as h   
  INNER JOIN sys.indexes as i  
            ON h.object_id = i.object_id  
           AND h.index_id  = i.index_id  
    INNER JOIN sys.memory_optimized_tables_internal_attributes ia ON h.xtp_object_id=ia.xtp_object_id
    INNER JOIN sys.tables t on h.object_id=t.object_id
  WHERE ia.type!=1
  ORDER BY [user_table], [internal_table_type], [index]; 
```

Beachten Sie, dass die BUCKET_COUNT des Indexes für interne Tabellen nicht geändert werden kann. Daher sollte die Ausgabe dieser Abfrage nur als informativ angesehen werden. Keine Aktion erforderlich.  

Bei dieser Abfrage wird davon ausgegangen, dass keine Zeilen zurückgegeben werden, es sei denn, Sie verwenden ein Feature, das Hash Indizes für interne Tabellen nutzt. Die folgende Speicher optimierte Tabelle enthält einen columnstore--Index. Nachdem Sie diese Tabelle erstellt haben, werden Hash Indizes für interne Tabellen angezeigt.

```sql
  CREATE TABLE dbo.table_columnstore
  (
    c1 INT NOT NULL PRIMARY KEY NONCLUSTERED,
    INDEX ix_columnstore CLUSTERED COLUMNSTORE
  ) WITH (MEMORY_OPTIMIZED=ON)
```

## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
