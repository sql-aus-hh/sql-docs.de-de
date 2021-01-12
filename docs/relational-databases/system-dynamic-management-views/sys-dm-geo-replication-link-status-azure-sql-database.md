---
description: sys.dm_geo_replication_link_status (Azure SQL-Datenbank)
title: sys.dm_geo_replication_link_status
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_link_status
- dm_geo_replication_link_status_TSQL
- sys.dm_geo_replication_link_status
- sys.dm_geo_replication_link_status_TSQL
helpviewer_keywords:
- dm_geo_replication_link_status dynamic management view
- sys.dm_geo_replication_link_status dynamic management view
ms.assetid: d763d679-470a-4c21-86ab-dfe98d37e9fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 2134a7890d23a7a080a576f8337e750bee2563eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092802"
---
# <a name="sysdm_geo_replication_link_status-azure-sql-database"></a>sys.dm_geo_replication_link_status (Azure SQL-Datenbank)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Enthält eine Zeile für jeden Replikations Link zwischen primären und sekundären Datenbanken in einer Partnerschaft für die georeplikation. Dies schließt primäre und sekundäre Datenbanken ein. Wenn für eine bestimmte primäre Datenbank mehrere Verknüpfungen für die fortlaufende Replikation vorhanden sind, enthält die Tabelle eine Zeile für jede der Beziehungen. Die Sicht wird in allen Datenbanken, einschließlich der logischen „master“-Datenbank, erstellt. Wenn aber diese Sicht in der logischen master-Datenbank abgerufen wird, wird ein leeres Set zurückgegeben.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|link_guid|**uniqueidentifier**|Eindeutige ID des Replikations Links.|  
|partner_server|**sysname**|Der Name des SQL-Datenbankservers, der die verknüpfte Datenbank enthält.|  
|partner_database|**sysname**|Name der Verbindungsdatenbank auf dem SQL-Datenbankverbindungsserver.|  
|last_replication|**datetimeoffset**|Der Zeitstempel der Bestätigung der letzten Transaktion durch den sekundären auf der Grundlage der primären datenbankuhr. Dieser Wert ist nur in der primären Datenbank verfügbar.|  
|replication_lag_sec|**int**|Zeitunterschied in Sekunden zwischen dem last_replication Wert und dem Zeitstempel des Commits der Transaktion auf der primären Datenbank basierend auf der primären Daten Bank Uhr.  Dieser Wert ist nur in der primären Datenbank verfügbar.|  
|replication_state|**tinyint**|Der Status der georeplikation für diese Datenbank, eine der folgenden:<br /><br /> 1 = Seeding. Für das Ziel der georeplikation wird ein Seeding durchgeführt, die beiden Datenbanken sind jedoch noch nicht synchronisiert. Bis das Seeding abgeschlossen ist, können Sie keine Verbindung mit der sekundären Datenbank herstellen. Durch das Entfernen der sekundären Datenbank aus der primären Datenbank wird der Seeding Vorgang abgebrochen.<br /><br /> 2 = Catch-up. Die sekundäre Datenbank befindet sich in einem Transaktions konsistenten Zustand und wird ständig mit der primären Datenbank synchronisiert.<br /><br /> 4 = angehalten. Dies ist keine aktive Beziehung mit kontinuierlichem Kopieren. Dieser Status gibt normalerweise an, dass die Bandbreite, die für den Interlink verfügbar ist, für die Ebene der Transaktionsaktivität in der primären Datenbank nicht ausreicht. Die Beziehung mit kontinuierlichem Kopieren ist jedoch nach wie vor intakt.|  
|replication_state_desc|**nvarchar(256)**|PENDING (AUSSTEHEND)<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|Rolle (role)|**tinyint**|Die georeplikationsrolle ist eine der folgenden:<br /><br /> 0 = primär. Der database_id bezieht sich auf die primäre Datenbank in der georeplikationspartnerschaft.<br /><br /> 1 = sekundär.  Der database_id bezieht sich auf die primäre Datenbank in der georeplikationspartnerschaft.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|Der sekundäre Typ, einer der folgenden:<br /><br /> 0 = Es sind keine direkten Verbindungen mit der sekundären Datenbank zulässig, und die Datenbank ist für den Lesezugriff nicht verfügbar.<br /><br /> 2 = alle Verbindungen sind für die Datenbank in der sekundären repl zulässig; ikation für schreibgeschützten Zugriff.|  
|secondary_allow_connections_desc|**nvarchar(256)**|Nein<br /><br /> Alle|  
|last_commit|**datetimeoffset**|Zeitpunkt der letzten Transaktion, für die ein Commit an die Datenbank ausgeführt wurde. Wenn die Datenbank in der primären Datenbank abgerufen wird, gibt Sie den Zeitpunkt des letzten Commit in der primären Datenbank an. Wenn Sie in der sekundären Datenbank abgerufen wird, gibt Sie den Zeitpunkt des letzten Commit in der sekundären Datenbank an. Wenn Sie in der sekundären Datenbank abgerufen werden, wenn der primäre Replikations Link nicht aktiv ist, wird angezeigt, bis der von der sekundären Datenbank aufgefasste Punkt erreicht wurde|
  
> [!NOTE]  
>  Wenn die Replikations Beziehung durch Entfernen der sekundären Datenbank (Abschnitt 4,2) beendet wird, wird die Zeile für diese Datenbank in der **sys.dm_geo_replication_link_status** Sicht nicht mehr angezeigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Alle Konten mit view_database_state Berechtigung können **sys.dm_geo_replication_link_status** Abfragen.  
  
## <a name="example"></a>Beispiel  
 Replikations Verzögerungen und Zeitpunkt der letzten Replikation der sekundären Datenbanken anzeigen.  
  
```  
SELECT   
     link_guid  
   , partner_server  
   , last_replication  
   , replication_lag_sec   
FROM sys.dm_geo_replication_link_status;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Alter Database &#40;Azure SQL-Datenbank&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.geo_replication_links &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [sys.dm_operation_status &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)   
 [sp_wait_for_database_copy_sync](../system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync.md)
