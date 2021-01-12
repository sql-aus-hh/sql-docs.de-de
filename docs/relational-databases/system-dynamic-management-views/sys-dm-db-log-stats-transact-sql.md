---
description: sys.dm_db_log_stats (Transact-SQL)
title: sys.dm_db_log_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_log_stats_TSQL
- sys.dm_db_log_stats
- sys.dm_db_log_stats_TSQL
- dm_db_log_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_stats dynamic management function
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ddfd608b3f2d5bb5c4c259dfb0ec822f245f72b3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096562"
---
# <a name="sysdm_db_log_stats-transact-sql"></a>sys.dm_db_log_stats (Transact-SQL)   
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Gibt Attribute der Übersichts Ebene und Informationen zu Transaktionsprotokoll Dateien von Datenbanken zurück. Verwenden Sie diese Informationen zur Überwachung und Diagnose der Integrität des Transaktions Protokolls.   
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
 sys.dm_db_log_stats ( database_id )
```  
  
## <a name="arguments"></a>Argumente  

*database_id* | NULL | **Standard**

Die ID der Datenbank. `database_id` ist `int`. Gültige Eingaben sind die ID einer Datenbank, `NULL` oder `DEFAULT` . Der Standardwert ist `NULL`. `NULL` und `DEFAULT` sind äquivalente Werte im Kontext der aktuellen Datenbank.  
Die integrierte [DB_ID](../../t-sql/functions/db-id-transact-sql.md)-Funktion kann angegeben werden. Wenn Sie `DB_ID` ohne Angabe eines Daten Banknamens verwenden, muss der Kompatibilitäts Grad der aktuellen Datenbank 90 oder größer sein.

  
## <a name="tables-returned"></a>Zurückgegebene Tabellen  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|database_id    |**int**    |Datenbank-ID |  
|recovery_model |**nvarchar(60)**   |   Wiederherstellungs Modell der Datenbank. Mögliche Werte sind: <br /> SIMPLE<br /> BULK_LOGGED <br /> FULL |  
|log_min_lsn    |**nvarchar(24)**   |   Aktuelle Start- [Protokoll Folge Nummer (Log Sequence Number, LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) im Transaktionsprotokoll.|  
|log_end_lsn    |**nvarchar(24)**   |   [Protokoll Sequenznummer (Log Sequence Number, LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) des letzten Protokolldaten Satzes im Transaktionsprotokoll.|  
|current_vlf_sequence_number    |**bigint** |   Aktuelle [VLF-Sequenznummer (Virtual Log File)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) zum Zeitpunkt der Ausführung.|  
|current_vlf_size_mb    |**float**  |   Aktuelle Größe der [virtuellen Protokolldatei (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) in MB.|   
|total_vlf_count    |**bigint** |   Gesamtanzahl der [virtuellen Protokolldateien (VLFs)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) im Transaktionsprotokoll. |  
|total_log_size_mb  |**float**  |   Gesamtgröße des Transaktions Protokolls in MB. |  
|active_vlf_count   |**bigint** |   Gesamtanzahl aktiver [virtueller Protokolldateien (VLFs)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) im Transaktionsprotokoll.|  
|active_log_size_mb |**float**  |   Gesamtgröße des aktiven Transaktions Protokolls in MB.|  
|log_truncation_holdup_reason   |**nvarchar(60)**   |   Grund für das Abschneiden der Protokoll Kürzung. Der Wert ist identisch  `log_reuse_wait_desc` mit der Spalte von `sys.databases` .  (Ausführlichere Erläuterungen zu diesen Werten finden Sie [im Transaktionsprotokoll](../../relational-databases/logs/the-transaction-log-sql-server.md).) <br />Mögliche Werte sind: <br />NOTHING<br />CHECKPOINT<br />LOG_BACKUP<br />ACTIVE_BACKUP_OR_RESTORE<br />ACTIVE_TRANSACTION<br />DATABASE_MIRRORING<br />REPLIKATION<br />DATABASE_SNAPSHOT_CREATION<br />LOG_SCAN<br />AVAILABILITY_REPLICA<br />OLDEST_PAGE<br />XTP_CHECKPOINT<br />anderer vorübergehender |  
|log_backup_time    |**datetime**   |   Zeitpunkt der letzten Sicherung des Transaktions Protokolls.|   
|log_backup_lsn |**nvarchar(24)**   |   Die letzte Protokoll Folge Nummer der Transaktionsprotokoll Sicherung [(Log Sequence Number, LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|   
|log_since_last_log_backup_mb   |**float**  |   Protokoll Größe in MB seit der letzten [Protokoll Folge Nummer (Log Sequence Number, LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)der Transaktionsprotokoll Sicherung.|  
|log_checkpoint_lsn |**nvarchar(24)**   |   Letzte Prüfpunkt- [Protokoll Folge Nummer (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_since_last_checkpoint_mb   |**float**  |   Protokoll Größe in MB seit der letzten Prüfpunkt- [Protokoll Folge Nummer (Log Sequence Number, LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_recovery_lsn   |**nvarchar(24)**   |   Wiederherstellungs- [Protokoll Folge Nummer (Log Sequence Number, LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) der Datenbank. Wenn `log_recovery_lsn` vor der Checkpoint-LSN auftritt, `log_recovery_lsn` ist die älteste aktive Transaktions-LSN, andernfalls die Prüfpunkt- `log_recovery_lsn` LSN.|  
|log_recovery_size_mb   |**float**  |   Protokoll Größe in MB seit Protokoll [Sequenznummer (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)für die Protokoll Wiederherstellung.|  
|recovery_vlf_count |**bigint** |   Die Gesamtanzahl der [virtuellen Protokolldateien (VLFs)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) , die wieder hergestellt werden sollen, wenn ein Failover oder ein Server Neustart aufgetreten ist. |  


## <a name="remarks"></a>Bemerkungen
Wenn eine `sys.dm_db_log_stats` Datenbank, die an einer Verfügbarkeits Gruppe teilnimmt, als sekundäres Replikat ausgeführt wird, wird nur eine Teilmenge der oben beschriebenen Felder zurückgegeben.  Derzeit werden nur `database_id` , `recovery_model` und `log_backup_time` zurückgegeben, wenn Sie für eine sekundäre Datenbank ausgeführt werden.   

## <a name="permissions"></a>Berechtigungen  
Erfordert die- `VIEW DATABASE STATE` Berechtigung in der Datenbank.   
  
## <a name="examples"></a>Beispiele  

### <a name="a-determining-databases-in-a-ssnoversion-instance-with-high-number-of-vlfs"></a>A. Ermitteln von Datenbanken in einer- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Instanz mit einer hohen Anzahl von VLFs   
Die folgende Abfrage gibt die Datenbanken mit mehr als 100 VLFs in den Protokolldateien zurück. Eine große Anzahl von VLFs kann sich auf die Daten Bank Start-, Wiederherstellungs-und Wiederherstellungszeit auswirken.

```sql  
SELECT name AS 'Database Name', total_vlf_count AS 'VLF count' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id) 
WHERE total_vlf_count  > 100;
```   

### <a name="b-determining-databases-in-a-ssnoversion-instance-with-transaction-log-backups-older-than-4-hours"></a>B. Ermitteln von Datenbanken in einer- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Instanz mit Transaktionsprotokoll Sicherungen, die älter als 4 Stunden sind   
Die folgende Abfrage bestimmt die Zeit der letzten Protokoll Sicherung für die Datenbanken in der-Instanz.

```sql  
SELECT name AS 'Database Name', log_backup_time AS 'last log backup time' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id); 
```

## <a name="see-also"></a>Weitere Informationen  
[Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
  
