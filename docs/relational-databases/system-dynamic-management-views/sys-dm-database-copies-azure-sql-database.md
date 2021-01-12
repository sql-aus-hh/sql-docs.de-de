---
description: sys.dm_database_copies (Azure SQL-Datenbank)
title: sys.dm_database_copies (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_database_copies_TSQL
- sys.dm_database_copies
- dm_database_copies
- sys.dm_database_copies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_database_copies
- sys.dm_database_copies
ms.assetid: d03d4657-86d1-4496-97e6-cc3bc292e0b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: ef0c92766cfb03f56dfc08a476cb7e81dbf4743d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095180"
---
# <a name="sysdm_database_copies-azure-sql-database"></a>sys.dm_database_copies (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Gibt Informationen über das Kopieren der Datenbank zurück.  
  
Verwenden Sie die [sys.geo_replication_links](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md) -oder [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) Sichten (in SQL-Datenbank V12 verfügbar), um Informationen zu den Links für die georeplikation zurückzugeben.
  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Die ID der aktuellen Datenbank in der `sys.databases`-Sicht.|  
|**start_date**|**datetimeoffset**|Die UTC-Zeit in einem regionalen [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Datencenter, zu der das Kopieren der Datenbank initiiert wurde.|  
|**modify_date**|**datetimeoffset**|Die UTC-Zeit im regionalen [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Datencenter, zu der das Kopieren der Datenbank abgeschlossen wurde. Die neue Datenbank ist ab diesem Zeitpunkt im Hinblick auf Transaktionen konsistent mit der primären Datenbank. Die Abschluss Informationen werden alle 1 Minute aktualisiert.<br /><br />Die UTC-Zeit, die das letzte Update des percent_complete Felds widerspiegelt.|  
|**percent_complete**|**real**|Der Prozentsatz der kopierten Bytes. Mögliche Werte liegen zwischen 0 und 100. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] wird nach einigen Fehlern wie Failovern u. U. automatisch weiter ausgeführt und startet das Kopieren der Datenbank erneut. In diesem Fall würde percent_complete bei 0 neu starten.|  
|**error_code**|**int**|Wenn der Wert größer als 0 ist, gibt der Code den Fehler an, der beim Kopieren aufgetreten ist. Der Wert ist gleich 0, wenn keine Fehler aufgetreten sind.|  
|**error_desc**|**nvarchar (4096)**|Beschreibung des Fehlers, der beim Kopieren aufgetreten ist.|  
|**error_severity**|**int**|Gibt 16 zurück, wenn das Kopieren der Datenbank fehlgeschlagen ist.|  
|**error_state**|**int**|Gibt 1 zurück, wenn der Kopiervorgang fehlgeschlagen ist.|  
|**copy_guid**|**uniqueidentifier**|Eindeutige ID des Kopiervorgangs.|  
|**partner_server**|**sysname**|Der Name des SQL-Datenbankservers, auf dem die Kopie erstellt wird.|  
|**partner_database**|**sysname**|Der Name der Daten Bank Kopie auf dem Partner Server.|  
|**replication_state**|**tinyint**|Der Status der Replikation für fortlaufenden Kopiervorgang für diese Datenbank. Gültige Werte:<br /><br /> 0 = ausstehend. Die Erstellung der Daten Bank Kopie ist geplant, aber die erforderlichen Vorbereitungsschritte sind noch nicht abgeschlossen oder werden vorübergehend durch das Seeding Kontingent blockiert.<br /><br /> 1 = Seeding. Die Datenbank, für die das Seeding durchgeführt wird, ist noch nicht vollständig mit der Quelldatenbank synchronisiert. In diesem Zustand kann keine Verbindung mit der Kopie hergestellt werden. Um den laufenden Seeding Vorgang abzubrechen, muss die Kopier Datenbank gelöscht werden.|  
|**replication_state_desc**|**nvarchar(256)**|Beschreibung von replication_state. Folgende Werte sind möglich:<br /><br /> PENDING (AUSSTEHEND)<br /><br /> SEEDING<br />|  
|**maximum_lag**|**int**|Reserviertes Feld.|  
|**is_continuous_copy**|**bit**|0 = gibt 0 zurück|  
|**is_target_role**|**bit**|0 = Quelldatenbank<br /><br /> 1 = Datenbank kopieren|  
|**is_interlink_connected**|bit|Reserviertes Feld.|  
|**is_offline_secondary**|bit|Reserviertes Feld.|  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Ansicht ist nur in der **Master** -Datenbank für den Prinzipal Anmelde Namen auf Serverebene verfügbar.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können die **sys.dm_database_copies** Ansicht in der **Master** -Datenbank der Quell-oder Ziel [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Server verwenden. Wenn das Kopieren der Datenbank erfolgreich abgeschlossen wurde und die neue Datenbank online geschaltet wird, wird die Zeile in der **sys.dm_database_copies** Ansicht automatisch entfernt.  
  
  
