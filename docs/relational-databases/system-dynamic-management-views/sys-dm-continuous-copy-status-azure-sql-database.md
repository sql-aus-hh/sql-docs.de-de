---
description: sys.dm_continuous_copy_status (Azure SQL-Datenbank)
title: sys.dm_continuous_copy_status
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_continuous_copy_status_TSQL
- dm_continuous_copy_status_TSQL
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
dev_langs:
- TSQL
helpviewer_keywords:
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
ms.assetid: 411b2e71-4421-4ef5-900d-5af068750899
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 94370b562160315e53feb46a83cbb68a848e064f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099968"
---
# <a name="sysdm_continuous_copy_status-azure-sql-database"></a>sys.dm_continuous_copy_status (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Gibt eine Zeile für jede Benutzerdatenbank (v11) zurück, die zurzeit an einer georeplikationsbeziehung mit fortlaufendem Kopiervorgang beteiligt ist. Wenn für eine bestimmte primäre Datenbank mehr als eine Beziehung mit kontinuierlichem Kopieren initiiert wird, enthält diese Tabelle für jede aktive sekundäre Datenbank eine Zeile.  
  
Bei Verwendung von SQL-Datenbank V12 sollten Sie [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) verwenden (da *sys.dm_continuous_copy_status* nur für v11 gilt).

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**copy_guid**|**uniqueidentifier**|Eindeutige ID der Replikatdatenbank.|  
|**partner_server**|**sysname**|Der Name des SQL-Datenbankverbindungsservers.|  
|**partner_database**|**sysname**|Name der Verbindungsdatenbank auf dem SQL-Datenbankverbindungsserver.|  
|**last_replication**|**datetimeoffset**|Der Zeitstempel der zuletzt durchgeführten replizierten Transaktion.|  
|**replication_lag_sec**|**int**|Der Zeitunterschied in Sekunden zwischen der aktuellen Zeit und dem Zeitstempel der letzten Transaktion in der primären Datenbank, für die erfolgreich ein Commit ausgeführt wurde und die von der aktiven sekundären Datenbank nicht bestätigt wurde.|  
|**replication_state**|**tinyint**|Der Status der Replikation für fortlaufenden Kopiervorgang für diese Datenbank. Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> 1: Seeding. Für das Replikationsziel, das einen inkonsistenten Transaktionsstatus aufweist, wird ein Seeding durchgeführt. Solange das Seeding noch nicht abgeschlossen wurde, können Sie keine Verbindung mit der aktiven sekundären Datenbank herstellen. <br />2: abfangen. Die aktive sekundäre Datenbank holt derzeit den Rückstand zur primären Datenbank auf und weist hinsichtlich der Transaktionen einen konsistenten Status auf.<br />3: erneutes Seeding. Für die aktive sekundäre Datenbank wird aufgrund eines nicht behebbaren Replikationsfehlers automatisch ein erneutes Seeding durchgeführt.<br />4: angehalten. Dies ist keine aktive Beziehung mit kontinuierlichem Kopieren. Dieser Status gibt normalerweise an, dass die Bandbreite, die für den Interlink verfügbar ist, für die Ebene der Transaktionsaktivität in der primären Datenbank nicht ausreicht. Die Beziehung mit kontinuierlichem Kopieren ist jedoch nach wie vor intakt.|  
|**replication_state_desc**|**nvarchar(256)**|Beschreibung von replication_state. Folgende Werte sind möglich:<br /><br /> SEEDING<br /><br /> CATCH_UP<br /><br /> RE_SEEDING<br /><br /> SUSPENDED|  
|**is_rpo_limit_reached**|**bit**|Wird immer auf 0 festgelegt.|  
|**is_target_role**|**bit**|0 = Quelle der Kopienbeziehung<br /><br /> 1 = Ziel der Kopienbeziehung|  
|**is_interlink_connected**|**bit**|1 = Interlink ist verbunden.<br /><br /> 0 = Interlink-Verbindung ist getrennt.|  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Abrufen von Daten ist die Mitgliedschaft in der Daten Bank Rolle **db_owner** erforderlich. Der dbo-Benutzer, Mitglieder der Daten Bank Rolle " **DBManager** " und der SA-Anmelde Name können diese Sicht ebenfalls Abfragen.  
  
## <a name="remarks"></a>Bemerkungen  
 Die **sys.dm_continuous_copy_status** Ansicht wird in der **Ressourcen** Datenbank erstellt und ist in allen Datenbanken, einschließlich der logischen Master, sichtbar. Wenn aber diese Sicht in der logischen master-Datenbank abgerufen wird, wird ein leeres Set zurückgegeben.  
  
 Wenn die fortlaufende Kopier Beziehung für eine Datenbank beendet wird, wird die Zeile für diese Datenbank in der **sys.dm_continuous_copy_status** Ansicht nicht mehr angezeigt.  
  
 Wie die **sys.dm_database_copies** Ansicht gibt **sys.dm_continuous_copy_status** den Status der fortlaufenden Kopier Beziehung wieder, in der die Datenbank entweder eine primäre oder aktive sekundäre Datenbank ist. Im Gegensatz zu **sys.dm_database_copies** enthält **sys.dm_continuous_copy_status** mehrere Spalten, die Details zu Vorgängen und Leistung bereitstellen. Zu diesen Spalten gehören **last_replication** und **replication_lag_sec**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_database_copies &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)   
 [Aktive Geo-Replication gespeicherte Prozeduren &#40;Transact-SQL-&#41;](../system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
