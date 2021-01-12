---
description: backupset (Transact-SQL)
title: Backup (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/24/2020
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupset
- backupset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupset system table
- backup media [SQL Server], backupset system table
- backup sets [SQL Server]
ms.assetid: 6ff79bbf-4acf-4f75-926f-38637ca8a943
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 87d509f0a49668bd354fd1132bb3501c6d2bdbcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098697"
---
# <a name="backupset-transact-sql"></a>backupset (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Enthält eine Zeile für jeden Sicherungssatz. Ein *Sicherungssatz* enthält die Sicherung aus einem einzelnen, erfolgreichen Sicherungsvorgang. Die Anweisungen RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY und RESTORE VERIFYONLY verarbeiten einen einzelnen Sicherungssatz innerhalb des Mediensatzes auf den angegebenen Sicherungsmedien.  
  
 Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Eindeutige Sicherungssatz-ID, die den Sicherungssatz identifiziert. Identität, Primärschlüssel.|  
|**backup_set_uuid**|**uniqueidentifier**|Eindeutige Sicherungssatz-ID, die den Sicherungssatz identifiziert.|  
|**media_set_id**|**int**|Eindeutige Mediensatz-ID, die den Mediensatz identifiziert, der den Sicherungssatz enthält. Verweist auf **Backup Mediaset (media_set_id)**.|  
|**first_family_number**|**tinyint**|Familiennummer des Mediums, bei dem der Sicherungssatz beginnt. Kann den Wert NULL haben.|  
|**first_media_number**|**smallint**|Mediennummer des Mediums, bei dem der Sicherungssatz beginnt. Kann den Wert NULL haben.|  
|**last_family_number**|**tinyint**|Familiennummer des Mediums, bei dem der Sicherungssatz endet. Kann den Wert NULL haben.|  
|**last_media_number**|**smallint**|Mediennummer des Mediums, bei dem der Sicherungssatz endet. Kann den Wert NULL haben.|  
|**catalog_family_number**|**tinyint**|Familiennummer des Mediums, das den Beginn des Sicherungssatzverzeichnisses enthält. Kann den Wert NULL haben.|  
|**catalog_media_number**|**smallint**|Mediennummer des Mediums, das den Beginn des Sicherungssatzverzeichnisses enthält. Kann den Wert NULL haben.|  
|**position**|**int**|Position des Sicherungssatzes, die in dem Wiederherstellungsvorgang zum Suchen nach dem geeigneten Sicherungssatz und den geeigneten Dateien verwendet wird. Kann den Wert NULL haben. Weitere Informationen finden Sie unter File in [Backup &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**expiration_date**|**datetime**|Datum und Uhrzeit des Zeitpunkts, zu dem die Gültigkeit des Sicherungssatzes endet. Kann den Wert NULL haben.|  
|**software_vendor_id**|**int**|ID des Softwareanbieters, der den Sicherungsmedienheader schreibt. Kann den Wert NULL haben.|  
|**name**|**nvarchar(128)**|Name des Sicherungssatzes. Kann den Wert NULL haben.|  
|**description**|**nvarchar(255)**|Beschreibung des Sicherungssatzes. Kann den Wert NULL haben.|  
|**user_name**|**nvarchar(128)**|Name des Benutzers, der den Sicherungsvorgang durchführt. Kann den Wert NULL haben.|  
|**software_major_version**|**tinyint**|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Hauptversionsnummer. Kann den Wert NULL haben.|  
|**software_minor_version**|**tinyint**|Nebenversionsnummer von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Kann den Wert NULL haben.|  
|**software_build_version**|**smallint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Buildnummer. Kann den Wert NULL haben.|  
|**time_zone**|**smallint**|Der Unterschied zwischen der Ortszeit (in der der Sicherungs Vorgang stattfindet) und der koordinierten Weltzeit (UTC) in 15-Minuten-Intervallen unter Verwendung der Zeitzoneninformationen zu dem Zeitpunkt, zu dem der Sicherungs Vorgang gestartet wurde. Die Werte können zwischen -48 und +48 (einschließlich) liegen. Durch den Wert 127 wird angegeben, dass die Abweichung nicht bekannt ist. So entspricht z. B. der Wert -20 der Eastern Standard Time (EST) bzw. einer Zeit, die fünf Stunden nach der UTC liegt. Kann den Wert NULL haben.|  
|**mtf_minor_version**|**tinyint**|Nebenversionsnummer von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format. Kann den Wert NULL haben.|  
|**first_lsn**|**numeric(25,0)**|Protokollfolgenummer des ersten oder ältesten Protokolldatensatzes im Sicherungssatz. Kann den Wert NULL haben.|  
|**last_lsn**|**numeric(25,0)**|Protokollfolgenummer des nächsten Protokolldatensatzes nach dem Sicherungssatz. Kann den Wert NULL haben.|  
|**checkpoint_lsn**|**numeric(25,0)**|Protokollfolgenummer des Protokolldatensatzes, bei dem die Wiederholung beginnen muss. Kann den Wert NULL haben.|  
|**database_backup_lsn**|**numeric(25,0)**|Protokollsequenznummer der neuesten vollständigen Datenbanksicherung. Kann den Wert NULL haben.<br /><br /> **database_backup_lsn** ist der "Begin of Check"-Wert, der ausgelöst wird, wenn die Sicherung gestartet wird. Diese LSN stimmt mit **first_lsn** überein, wenn die Sicherung durchgeführt wird, wenn sich die Datenbank im Leerlauf befindet und keine Replikation konfiguriert ist.|  
|**database_creation_date**|**datetime**|Datum und Uhrzeit des ursprünglichen Erstellens der Datenbank. Kann den Wert NULL haben.|  
|**backup_start_date**|**datetime**|Datum und Uhrzeit des Beginns des Sicherungsvorgangs. Kann den Wert NULL haben.|  
|**backup_finish_date**|**datetime**|Datum und Uhrzeit des Endes des Sicherungsvorgangs. Kann den Wert NULL haben.|  
|**type**|**char(1)**|Art der Sicherung. Mögliche Werte sind:<br /><br /> D = Datenbank<br /><br /> I = Datenbank differenziell<br /><br /> L = Protokoll<br /><br /> F = Datei oder Dateigruppe<br /><br /> G = Datei differenziell<br /><br /> P = Teilweise<br /><br /> Q = Teilweise differenziell<br /><br /> Kann den Wert NULL haben.|  
|**sort_order**|**smallint**|Sortierreihenfolge des Servers, der den Sicherungsvorgang durchführt. Kann den Wert NULL haben. Weitere Informationen zu Sortier Reihenfolgen und Sortierungen finden Sie [unter Sortierung und Unicode-Unterstützung](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**Codepage**|**smallint**|Codepage des Servers, der den Sicherungsvorgang durchführt. Kann den Wert NULL haben. Weitere Informationen zu Codepages finden Sie [unter Sortierung und Unicode-Unterstützung](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**compatibility_level**|**tinyint**|Einstellung des Kompatibilitätsgrades für die Datenbank. Mögliche Werte sind:<br /><br /> 90 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 100 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 110 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 120 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> Kann den Wert NULL haben.<br /><br /> Weitere Informationen zu den Kompatibilitätsgraden finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**database_version**|**int**|Versionsnummer der Datenbank. Kann den Wert NULL haben.|  
|**backup_size**|**numeric(20,0)**|Größe des Sicherungssatzes in Bytes. Kann den Wert NULL haben. Bei VSS-Sicherungen ist backup_size ein geschätzter Wert.|  
|**database_name**|**nvarchar(128)**|Name der an dem Sicherungsvorgang beteiligten Datenbank. Kann den Wert NULL haben.|  
|**server_name**|**nvarchar(128)**|Name des Servers, der den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherungsvorgang ausführt. Kann den Wert NULL haben.|  
|**machine_name**|**nvarchar(128)**|Name des Computers, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgeführt wird. Kann den Wert NULL haben.|  
|**flags**|**int**|In wurde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die **Flags** -Spalte als veraltet markiert und durch die folgenden Bitspalten ersetzt:<br /><br /> **has_bulk_logged_data** <br /> **is_snapshot** <br /> **is_readonly** <br /> **is_single_user** <br /> **has_backup_checksums** <br /> **is_damaged** <br /> **begins_log_chain** <br /> **has_incomplete_metadata** <br /> **is_force_offline** <br /> **is_copy_only**<br /><br /> Kann den Wert NULL haben.<br /><br /> In Sicherungssätzen früherer Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] haben die Flagbits folgende Bedeutung:<br />1 = Die Sicherung enthält minimal protokollierte Daten. <br />2 = WITH SNAPSHOT wurde verwendet. <br />4 = Die Datenbank war zum Zeitpunkt der Sicherung schreibgeschützt.<br />8 = Die Datenbank befand sich zum Zeitpunkt der Sicherung im Einzelbenutzermodus.|  
|**unicode_locale**|**int**|Unicode-Gebietsschema. Kann den Wert NULL haben.|  
|**unicode_compare_style**|**int**|Unicode-Vergleichsstil. Kann den Wert NULL haben.|  
|**collation_name**|**nvarchar(128)**|Sortierungsname. Kann den Wert NULL haben.|  
|**Is_password_protected**|**bit**|Gibt an, ob der Sicherungssatz<br /><br /> kennwortgeschützt ist:<br /><br /> 0 = Nicht geschützt<br /><br /> 1 = Geschützt|  
|**recovery_model**|**nvarchar(60)**|Wiederherstellungsmodell für die Datenbank:<br /><br /> FULL<br /><br /> BULK-LOGGED<br /><br /> SIMPLE|  
|**has_bulk_logged_data**|**bit**|1 = Die Sicherung enthält massenprotokollierte Daten.|  
|**is_snapshot**|**bit**|1 = Die Sicherung wurde mithilfe der Option SNAPSHOT erstellt.|  
|**is_readonly**|**bit**|1 = Die Datenbank war zum Zeitpunkt der Sicherung schreibgeschützt.|  
|**is_single_user**|**bit**|1 = Die Datenbank befand sich zum Zeitpunkt der Sicherung im Einzelbenutzermodus.|  
|**has_backup_checksums**|**bit**|1 = Die Sicherung enthält Sicherungsprüfsummen.|  
|**is_damaged**|**bit**|1 = Beim Erstellen dieser Sicherung wurde eine Beschädigung der Datenbank festgestellt. Der Sicherungsvorgang sollte jedoch trotz der Fehler fortgesetzt werden.|  
|**begins_log_chain**|**bit**|1 = Die erste in einer kontinuierlichen Kette von Protokollsicherungen. Eine Protokollkette beginnt mit der ersten Protokollsicherung, die erstellt wurde, nachdem die Datenbank angelegt wurde oder nachdem ein Wechsel vom einfachen zum vollständigen oder massenprotokollierten Wiederherstellungsmodell erfolgt ist.|  
|**has_incomplete_metadata**|**bit**|1 = Eine Sicherung des Protokollfragments mit unvollständigen Metadaten. Weitere Informationen finden Sie unter [Protokollfragmentsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).|  
|**is_force_offline**|**bit**|1 = Beim Erstellen der Sicherung wurde die Datenbank mithilfe der Option NORECOVERY offline geschaltet.|  
|**is_copy_only**|**bit**|1 = Eine Kopiesicherung. Weitere Informationen finden Sie unter [Kopiesicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).|  
|**first_recovery_fork_guid**|**uniqueidentifier**|ID des ersten Wiederherstellungs-Verzweigungspunkts. Dies entspricht **FirstRecoveryForkID** von RESTORE HEADERONLY.<br /><br /> Für Datensicherungen **first_recovery_fork_guid** **last_recovery_fork_guid**.|  
|**last_recovery_fork_guid**|**uniqueidentifier**|ID des letzten Wiederherstellungs-Verzweigungspunkts. Dies entspricht wieder **herstellungsforkid** von RESTORE HEADERONLY.<br /><br /> Für Datensicherungen **first_recovery_fork_guid** **last_recovery_fork_guid**.|  
|**fork_point_lsn**|**numeric(25,0)**|Wenn **first_recovery_fork_guid** nicht gleich **last_recovery_fork_guid** ist, ist dies die Protokoll Folge Nummer des Verzweigungs Punkts. Andernfalls ist der Wert NULL.|  
|**database_guid**|**uniqueidentifier**|Eindeutige ID für die Datenbank. Dies entspricht **BindingId** von RESTORE HEADERONLY. Wenn die Datenbank wiederhergestellt wird, wird ein neuer Wert zugewiesen.|  
|**family_guid**|**uniqueidentifier**|Eindeutige ID der ursprünglichen Datenbank zum Zeitpunkt der Erstellung. Dieser Wert bleibt unverändert, wenn die Datenbank wiederhergestellt wird, und zwar auch dann, wenn sie mit einem anderen Namen wiederhergestellt wird.|  
|**differential_base_lsn**|**numeric(25,0)**|Basis-LSN für differenzielle Sicherungen. Bei einer Einzel basierten differenziellen Sicherung Änderungen mit LSNs, die größer oder gleich **differential_base_lsn** sind, sind in der differenziellen Sicherung enthalten.<br /><br /> Bei einem multibasierten differenziellen Wert ist der Wert NULL, und die Basis-LSN muss auf Dateiebene bestimmt werden (siehe [Backup File &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)).<br /><br /> Bei nicht differenziellen Sicherungstypen ist der Wert immer NULL.|  
|**differential_base_guid**|**uniqueidentifier**|Bei einer differenziellen Sicherung auf der Basis einer Sicherung entspricht dieser Wert dem eindeutigen Bezeichner der differenziellen Basis.<br /><br /> Bei einer differenziellen Sicherung auf der Basis mehrerer Sicherungen ist der Wert NULL, und die Basis für die differenzielle Sicherung muss auf Dateiebene bestimmt werden.<br /><br /> Bei nicht differenziellen Sicherungstypen ist der Wert NULL.|  
|**compressed_backup_size**|**Numerisch (20, 0)**|Gesamtbytezahl der auf einem Datenträger gespeicherten Sicherung.<br /><br /> Verwenden Sie zum Berechnen des Komprimierungs Verhältnisses **compressed_backup_size** und **backup_size**.<br /><br /> Während eines **msdb** -Upgrades wird dieser Wert auf NULL festgelegt. Dies gibt eine nicht komprimierte Sicherung an.|  
|**key_algorithm**|**nvarchar(32)**|Der Verschlüsselungsalgorithmus, der zum Verschlüsseln der Sicherung verwendet wird. Der NO_Encryption-Wert gab an, dass die Sicherung nicht verschlüsselt wurde.|  
|**encryptor_thumbprint**|**varbinary(20)**|Der Fingerabdruck der Verschlüsselung, der verwendet werden kann, um das Zertifikat oder den asymmetrischen Schlüssel in der Datenbank zu ermitteln. Wenn die Sicherung nicht verschlüsselt wurde, ist dieser Wert NULL.|  
|**encryptor_type**|**nvarchar(32)**|Verwendeter Verschlüsselungstyp: Zertifikat oder asymmetrischer Schlüssel. . Wenn die Sicherung nicht verschlüsselt wurde, ist dieser Wert NULL.|  
  
## <a name="remarks"></a>Bemerkungen
- RESTORE VERIFYONLY FROM *backup_device* mit LOADHISTORY füllt die Spalte der **Backup Mediaset** -Tabelle mit den entsprechenden Werten aus dem Medien Satz Header auf.  
- Führen Sie die gespeicherte Prozedur [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) aus, um die Anzahl der Zeilen in dieser Tabelle und in anderen Sicherungs-und Verlaufs Tabellen zu verringern.  
- Für SQL verwaltete Instanz wird in der Tabelle mit den Sicherungskopien nur der Sicherungs Verlauf für vom Benutzer initiierte [Kopiesicherungen](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)angezeigt. In der Tabelle mit den Sicherungskopien wird der Sicherungs Verlauf für automatische Sicherungen, die vom Dienst ausgeführt werden, nicht angezeigt. 
  
## <a name="see-also"></a>Weitere Informationen  
 [Sichern und Wiederherstellen von Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [Mögliche Medienfehler während der Sicherung und Wiederherstellung &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [Mediensätze, Medienfamilien und Sicherungssätze &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Wiederherstellungsmodelle &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Sichern und Wiederherstellen von Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)  
  
  
