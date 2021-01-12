---
description: sys.external_data_sources (Transact-SQL)
title: sys.external_data_sources (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1016db6e-9950-4ae2-a004-bd4171e27359
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6558dda3bf0cb87f3cdfc14b3beda0fc2c329557
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094615"
---
# <a name="sysexternal_data_sources-transact-sql"></a>sys.external_data_sources (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jede externe Datenquelle in der aktuellen Datenbank für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
 Enthält eine Zeile für jede externe Datenquelle auf dem Server für [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|data_source_id|**int**|Objekt-ID für die externe Datenquelle.||  
|name|**sysname**|Der Name der externen Datenquelle.||  
|location|**nvarchar(4000)**|Die Verbindungs Zeichenfolge, die das Protokoll, die IP-Adresse und den Port für die externe Datenquelle enthält.||  
|type_desc|**nvarchar(255)**|Der als Zeichenfolge angezeigte Daten Quellentyp.|Hadoop, RDBMS, SHARD_MAP_MANAGER, remotedataarchivetypeer-DataSource|  
|type|**tinyint**|Der als Zahl angezeigte Daten Quellentyp.|0-Hadoop<br /><br /> 1-RDBMS<br /><br /> 2-SHARD_MAP_MANAGER<br /><br /> 3-remotedataarchivetypeer-DataSource|  
|resource_manager_location|**nvarchar(4000)**|Für den Typ Hadoop, den IP-und Port Speicherort des Hadoop-Ressourcen-Managers. Diese wird zum Übermitteln eines Auftrags in einer Hadoop-Datenquelle verwendet.<br /><br /> NULL für andere Typen externer Datenquellen.||  
|credential_id|**int**|Die Objekt-ID der Daten Bank weit gültigen Anmelde Informationen, die zum Herstellen einer Verbindung mit der externen Datenquelle verwendet werden.||  
|database_name|**sysname**|Für den Typ RDBMS der Name der Remote Datenbank. Für Type, SHARD_MAP_MANAGER, der Name der shardzuordnungs-Manager-Datenbank. NULL für andere Typen externer Datenquellen.||  
|shard_map_name|**sysname**|Für Type SHARD_MAP_MANAGER der Name der shardzuordnung. NULL für andere Typen externer Datenquellen.||  
  
## <a name="permissions"></a>Berechtigungen  
 Die Sichtbarkeit der Metadaten in Katalogsichten ist auf sicherungsfähige Elemente eingeschränkt, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.external_file_formats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_tables &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
