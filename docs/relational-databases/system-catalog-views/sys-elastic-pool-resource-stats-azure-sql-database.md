---
description: sys.elastic_pool_resource_stats (Azure SQL-Datenbank)
title: sys.elastic_pool_resource_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.elastic_pool_resource_stats catalog view
helpviewer_keywords:
- sys.elastic_pool_resource_stats_TSQL
- sys.elastic_pool_resource_stats
- elastic_pool_resource_stats_TSQL
- elastic_pool_resource_stats
ms.assetid: f242c1bd-3cc8-4c8b-8aaf-c79b6a8a0329
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1b97fe07119f28211186e2efa3986308af477947
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102771"
---
# <a name="syselastic_pool_resource_stats-azure-sql-database"></a>sys.elastic_pool_resource_stats (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Gibt Ressourcenverwendungsstatistiken für alle Pools für elastische Datenbanken in einem SQL-Datenbank-Server zurück. Für jeden Pool für elastische Datenbanken ist eine Zeile pro 15-Sekunden-Berichtzeitfenster vorhanden (vier Zeilen pro Minute). Dies umfasst CPU-, E/A-, Protokoll-, Speicher- und gleichzeitige Anforderungs-/Sitzungsauslastung durch alle Datenbanken im Pool. Diese Daten werden 14 Tage lang aufbewahrt. 
  
||  
|-|  
|**Gilt für**:  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] V12.|  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime2**|Die UTC-Zeit, die den Beginn des 15-Sekunden-Berichts Intervalls angibt.|  
|**end_time**|**datetime2**|Die UTC-Zeit, die das Ende des 15-Sekunden-Berichts Intervalls angibt.|  
|**elastic_pool_name**|**nvarchar(128)**|Der Name des Pools für elastische Datenbanken.|  
|**avg_cpu_percent**|**Dezimalzahl (5, 2)**|Durchschnittliche Computeauslastung als Prozentwert der maximalen Kapazität des Pools.|  
|**avg_data_io_percent**|**Dezimalzahl (5, 2)**|Durchschnittliche E/A-Auslastung als Prozentwert der maximalen Kapazität des Pools.|  
|**avg_log_write_percent**|**Dezimalzahl (5, 2)**|Durchschnittliche Auslastung der Schreibressourcen als Prozentwert der maximalen Kapazität des Pools.|  
|**avg_storage_percent**|**Dezimalzahl (5, 2)**|Durchschnittliche Speicherauslastung als Prozentwert der Speicherbeschränkung des Pools.|  
|**max_worker_percent**|**Dezimalzahl (5, 2)**|Maximale Anzahl der gleichzeitigen Worker (Anforderungen) als Prozentwert basierend auf der maximalen Kapazität des Pools.|  
|**max_session_percent**|**Dezimalzahl (5, 2)**|Maximale Anzahl der gleichzeitigen Sitzungen als Prozentwert basierend auf der maximalen Kapazität des Pools.|  
|**elastic_pool_dtu_limit**|**int**|Aktuelle maximale DTU-Einstellung des Pools für elastische Datenbanken für einen bestimmten Pool für elastische Datenbanken und ein bestimmtes Intervall.|  
|**elastic_pool_storage_limit_mb**|**bigint**|Aktuelle maximale Speicherbeschränkung des Pools für elastische Datenbanken für einen bestimmten Pool für elastische Datenbanken und ein bestimmtes Intervall in MB.|
|**avg_allocated_storage_percent**|**Dezimalzahl (5, 2)**|Der Prozentsatz des Daten Speicherplatzes, der von allen Datenbanken im elastischen Pool zugewiesen wird.  Dies ist das Verhältnis des Daten Speicherplatzes, der der maximalen Datengröße für den Pool für elastische Datenbanken zugeordnet ist.  Weitere Informationen finden Sie [unter: Dateispeicher Platz Verwaltung in SQL-Datenbank](/azure/sql-database/sql-database-file-space-management)|  
  
## <a name="remarks"></a>Bemerkungen

 Diese Sicht ist in der Master-Datenbank des SQL-Datenbankservers vorhanden. Sie müssen mit der Master-Datenbank verbunden sein, um **sys.elastic_pool_resource_stats** abzufragen.  
  
## <a name="permissions"></a>Berechtigungen

 Erfordert die Mitgliedschaft in der **DBManager** -Rolle.  
  
## <a name="examples"></a>Beispiele

 Im folgenden Beispiel werden Ressourcen Verwendungs Daten zurückgegeben, die nach dem letzten Zeitpunkt für alle Pools für elastische Datenbanken im aktuellen SQL-Datenbankserver geordnet sind.  
  
```sql
SELECT * FROM sys.elastic_pool_resource_stats
ORDER BY end_time DESC;  
```

 Im folgenden Beispiel wird der durchschnittliche prozentuale DTU-Verbrauch für einen bestimmten Pool berechnet.  

```sql
SELECT start_time, end_time,
  (SELECT Max(v)
FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS value(v)) AS [avg_DTU_percent]
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = '<your pool name>'
ORDER BY end_time DESC;  
```

## <a name="see-also"></a>Weitere Informationen

 [Zämes explocwachstum mit elastischen Datenbanken](/azure/azure-sql/database/elastic-pool-overview)   
 [Erstellen und Verwalten eines Pools für elastische SQL-Datenbank-Datenbanken](/azure/azure-sql/database/elastic-pool-overview)   
 [sys.resource_stats &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [sys.dm_db_resource_stats &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)  
  
