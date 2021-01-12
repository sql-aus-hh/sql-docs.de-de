---
description: sys.external_tables (Transact-SQL)
title: sys.external_tables (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: fac4720c-b679-4ab2-864b-ff7810a9b559
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 443086b2deb37b8c1cce1170e2869fe09db81560
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098349"
---
# <a name="sysexternal_tables-transact-sql"></a>sys.external_tables (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jede externe Tabelle in der aktuellen Datenbank.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Eine Liste der Spalten, die diese Sicht erbt, finden Sie unter [sys. Objects &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).||  
|max_column_id_used|**int**|Maximale Spalten-ID, die jemals für diese Tabelle verwendet wurde.||  
|uses_ansi_nulls|**bit**|Beim Erstellen der Tabelle war die Datenbankoption SET ANSI_NULLS auf ON festgelegt.||  
|data_source_id|**int**|Objekt-ID für die externe Datenquelle.||  
|file_format_id|**int**|Bei externen Tabellen über eine externe Hadoop-Datenquelle handelt es sich hierbei um die Objekt-ID für das externe Dateiformat.||  
|location|**nvarchar(4000)**|Bei externen Tabellen über eine externe Hadoop-Datenquelle handelt es sich hierbei um den Pfad der externen Daten in HDFS.||  
|reject_type|**tinyint**|Bei externen Tabellen über eine externe Hadoop-Datenquelle ist dies die Art und Weise, wie abgelehnte Zeilen beim Abfragen externer Daten gezählt werden.|Value: die Anzahl der abgelehnten Zeilen.<br /><br /> Prozentsatz: der Prozentsatz der abgelehnten Zeilen.|  
|reject_value|**float**|Für externe Tabellen über eine externe Hadoop-Datenquelle:<br /><br /> Bei *reject_type =* value ist dies die Anzahl der Zeilen Umschreibungen, die zulässig sind, bevor ein Fehler bei der Abfrage auftritt.<br /><br /> Bei *reject_type* = Prozentsatz ist dies der Prozentsatz der Zeilen Umschreibungen, die zugelassen werden, bevor die Abfrage fehlschlägt.||  
|reject_sample_value|**int**|Bei *reject_type* = Prozentsatz ist dies die Anzahl der Zeilen, die entweder erfolgreich oder erfolglos geladen werden sollen, bevor der Prozentsatz der abgelehnten Zeilen berechnet wird.|NULL, wenn reject_type = Value.|  
|distribution_type|**int**|Bei externen Tabellen über eine SHARD_MAP_MANAGER externe Datenquelle handelt es sich hierbei um die Datenverteilung der Zeilen in den zugrunde liegenden Basistabellen.|0-Sharding<br /><br /> 1-repliziert<br /><br /> 2-Roundrobin|  
|distribution_desc|**nvarchar(120)**|Bei externen Tabellen über eine SHARD_MAP_MANAGER externe Datenquelle ist dies der Verteilungstyp, der als Zeichenfolge angezeigt wird.||  
|sharding_column_id|**int**|Bei externen Tabellen über eine SHARD_MAP_MANAGER externe Datenquelle und eine shardverteilung ist dies die Spalten-ID der Spalte, die die shardingschlüsselwerte enthält.||  
|remote_schema_name|**sysname**|Bei externen Tabellen über eine SHARD_MAP_MANAGER externe Datenquelle ist dies das Schema, in dem sich die Basistabelle auf den Remote Datenbanken befindet (wenn Sie sich von dem Schema unterscheidet, in dem die externe Tabelle definiert ist).||  
|remote_object_name|**sysname**|Bei externen Tabellen über eine SHARD_MAP_MANAGER externe Datenquelle ist dies der Name der Basistabelle in den Remote Datenbanken (wenn Sie sich vom Namen der externen Tabelle unterscheidet).||  
  
## <a name="permissions"></a>Berechtigungen  
 Die Sichtbarkeit der Metadaten in Katalogsichten ist auf sicherungsfähige Elemente eingeschränkt, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.external_file_formats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_data_sources &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  
