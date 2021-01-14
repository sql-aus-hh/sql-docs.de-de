---
title: sys.dm_db_xtp_checkpoint_files (Transact-SQL) | Microsoft-Dokumentation
description: Zeigt Informationen zu Prüf Punkt Dateien an, einschließlich Dateigröße, physischer Speicherort und Transaktions-ID. Erfahren Sie, wie sich diese Sicht bei Versionen von SQL Server unterscheidet.
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d94ff0799552f1d6c89e30e3b2712942ed0d4177
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171412"
---
# <a name="sysdm_db_xtp_checkpoint_files-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Zeigt Informationen zu Prüfpunktdateien, einschließlich Dateigröße, physischem Speicherort und der Transaktions-ID an.  
  
> **Hinweis:** Für den aktuellen Prüfpunkt, der nicht geschlossen wurde, wird die Zustands Spalte von s `ys.dm_db_xtp_checkpoint_files` für neue Dateien erstellt. Ein Prüfpunkt wird automatisch geschlossen, wenn das Wachstum des Transaktions Protokolls seit dem letzten Prüfpunkt ausreicht, oder wenn Sie den `CHECKPOINT` Befehl ([Checkpoint &#40;Transact-SQL-&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)) ausgeben.  
  
 Eine Speicher optimierte Datei Gruppe verwendet intern nur-Anhängen-Dateien, um eingefügte und gelöschte Zeilen für in-Memory-Tabellen zu speichern. Es gibt zwei Typen von Dateien. Eine Datendatei enthält eingefügte Zeilen, während eine Delta Datei Verweise auf gelöschte Zeilen enthält. 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] unterscheidet sich wesentlich von neueren Versionen und wird weiter unten im Thema unter [SQL Server 2014](#bkmk_2014)erläutert.  
  
 Weitere Informationen finden Sie unter [Erstellen und Verwalten von Speicher für Memory-Optimized Objekte](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
##  <a name="sssql15-and-later"></a><a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher  
 In der folgenden Tabelle werden die Spalten für `sys.dm_db_xtp_checkpoint_files` , beginnend mit, beschrieben **[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]** .  
  
|Spaltenname|type|Beschreibung|  
|-----------------|----------|-----------------|  
|container_id|**int**|Die ID des Containers (in sys.database_files als Datei vom Typ FILESTREAM dargestellt), dem die Daten- oder Änderungsdatei angehört. Joins mit file_id in [sys.database_files &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID des Containers, zu dem die Stamm-, Daten-oder Änderungsdatei gehört. Joins mit file_guid in der sys.database_files Tabelle.|  
|checkpoint_file_id|**uniqueidentifier**|GUID der Prüf Punkt Datei.|  
|relative_file_path|**nvarchar(256)**|Der Pfad der Datei relativ zum Container, dem Sie zugeordnet ist.|  
|file_type|**smallint**|-1 kostenlos<br /><br /> 0 für die Datendatei.<br /><br /> 1 für Delta Datei.<br /><br /> 2 für Stammdatei<br /><br /> 3 für große Datendateien|  
|file_type_desc|**nvarchar(60)**|Free: alle Dateien, die als frei verwaltet werden, sind für die Zuordnung verfügbar. Die Größe der kostenlosen Dateien kann je nach den erwarteten Anforderungen des Systems variieren. Die maximale Größe beträgt 1 GB.<br /><br /> Daten-Datendateien enthalten Zeilen, die in Speicher optimierte Tabellen eingefügt wurden.<br /><br /> Delta-Delta Dateien enthalten Verweise auf Zeilen in Datendateien, die gelöscht wurden.<br /><br /> Stamm Stammdateien enthalten System Metadaten für Speicher optimierte und nativ kompilierte Objekte.<br /><br /> Große Daten große Datendateien enthalten Werte, die in (n) varchar (max)-und varbinary (max)-Spalten eingefügt werden, sowie die Spalten Segmente, die Teil von columnstore--Indizes für Speicher optimierte Tabellen sind.|  
|internal_storage_slot|**int**|Der Index der Datei im internen Speicherarray. NULL für root oder für einen anderen Status als 1.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Die entsprechende Daten-oder Änderungsdatei. NULL für root.|  
|file_size_in_bytes|**bigint**|Größe der Datei auf dem Datenträger.|  
|file_size_used_in_bytes|**bigint**|Bei Prüfpunktdateipaaren, die noch mit Daten aufgefüllt werden, wird diese Spalte nach dem nächsten Prüfpunkt aktualisiert.|  
|logical_row_count|**bigint**|Für Daten, die Anzahl der eingefügten Zeilen.<br /><br /> Für Delta die Anzahl der Zeilen, die nach der Erfassung von DROP TABLE gelöscht wurden.<br /><br /> For root, NULL.|  
|state|**smallint**|0-vorab erstellt<br /><br /> 1-Unterkonstruktion<br /><br /> 2 - ACTIVE<br /><br /> 3-Merge-Ziel<br /><br /> 8: warten auf Protokoll Verkürzung|  
|state_desc|**nvarchar(60)**|Vorab erstellt: eine Reihe von Prüf Punkt Dateien wird vorab zugeordnet, um Wartezeiten für die Zuordnung neuer Dateien während der Ausführung von Transaktionen zu minimieren oder zu eliminieren. Diese vorab erstellten Dateien können je nach den geschätzten Anforderungen der Arbeitsauslastung variieren, Sie enthalten jedoch keine Daten. Dies ist ein Speicher Aufwand in Datenbanken mit einer MEMORY_OPTIMIZED_DATA-Datei Gruppe.<br /><br /> Bei der Erstellung werden diese Prüf Punkt Dateien erstellt. Dies bedeutet, dass Sie basierend auf den von der Datenbank generierten Protokolldaten Sätzen aufgefüllt werden und noch nicht Teil eines Prüf Punkts sind.<br /><br /> Aktiv: Diese enthalten die eingefügten/gelöschten Zeilen aus vorherigen geschlossenen Prüfpunkten. Sie enthalten den Inhalt der Tabellen, die in den Arbeitsspeicher eingelesen werden, bevor der aktive Teil des Transaktions Protokolls beim Neustart der Datenbank angewendet wird. Wir gehen davon aus, dass die Größe dieser Prüf Punkt Dateien ungefähr 2X der Speicher optimierten Tabellen im Arbeitsspeicher entspricht, vorausgesetzt, dass der Merge-Vorgang die transaktionale Arbeitsauslastung beibehält.<br /><br /> Merge target: das Ziel von Merge-Vorgängen: in diesen Prüf Punkt Dateien werden die konsolidierten Daten Zeilen aus den Quelldateien gespeichert, die durch die Merge-Richtlinie identifiziert wurden. Nachdem die Zusammenführung installiert wurde, befinden sich die MERGE TARGET-Übergänge im Status ACTIVE.<br /><br /> Warten auf Protokoll abkürzen: Nachdem die Zusammenführung installiert wurde und das Merge-Ziel-cFP Teil eines permanenten Prüf Punkts ist, wechseln die Prüf Punkt Dateien für die Zusammenführung des Quell Prüf Punkts in diesen Zustand. Dateien in diesem Zustand sind erforderlich, um die korrekte Betriebs gemäße Richtigkeit der Datenbank mit Speicher optimierter Tabelle zu tun.  Dies gilt beispielsweise für die Wiederherstellung von einem dauerhaften Prüfpunkt, um zu einem früheren Zustand zurückzukehren.|  
|lower_bound_tsn|**bigint**|Untere Grenze der Transaktion in der Datei. NULL, wenn der Zustand nicht in (1, 3) liegt.|  
|upper_bound_tsn|**bigint**|Obere Grenze der Transaktion in der Datei. NULL, wenn der Zustand nicht in (1, 3) liegt.|  
|begin_checkpoint_id|**bigint**|ID des BEGIN-Prüf Punkts.|  
|end_checkpoint_id|**bigint**|ID des Endpunkts.|  
|last_updated_checkpoint_id|**bigint**|ID des letzten Prüf Punkts, von dem diese Datei aktualisiert wurde.|  
|encryption_status|**smallint**|0, 1, 2|  
|encryption_status_desc|**nvarchar(60)**|0 => unverschlüsselt<br /><br /> 1 => mit Schlüssel 1 verschlüsselt<br /><br /> 2 => mit Schlüssel 2 verschlüsselt. Nur für aktive Dateien gültig.|  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 In der folgenden Tabelle werden die Spalten für `sys.dm_db_xtp_checkpoint_files` , für beschrieben **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Spaltenname|type|Beschreibung|  
|-----------------|----------|-----------------|  
|container_id|**int**|Die ID des Containers (in sys.database_files als Datei vom Typ FILESTREAM dargestellt), dem die Daten- oder Änderungsdatei angehört. Joins mit file_id in [sys.database_files &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|Die GUID des Containers, dem die Daten- oder Änderungsdatei angehört.|  
|checkpoint_file_id|**GUID**|Die ID der Daten- oder Änderungsdatei.|  
|relative_file_path|**nvarchar(256)**|Der Pfad zu der Daten- oder Änderungsdatei relativ zum Speicherort des Containers.|  
|file_type|**tinyint**|0 für die Datendatei.<br /><br /> 1 für die Änderungsdatei.<br /><br /> NULL, wenn die Zustandsspalte auf 7 festgelegt ist.|  
|file_type_desc|**nvarchar(60)**|Der Dateityp: DATA_FILE, DELTA_FILE oder NULL, wenn die Zustands Spalte auf 7 festgelegt ist.|  
|internal_storage_slot|**int**|Der Index der Datei im internen Speicherarray. NULL, wenn die Zustandsspalte auf 2 oder 3 festgelegt ist.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Die entsprechende Daten- oder Änderungsdatei.|  
|file_size_in_bytes|**bigint**|Die Größe der Datei, die verwendet wird. NULL, wenn die Zustandsspalte auf 5, 6 oder 7 festgelegt ist.|  
|file_size_used_in_bytes|**bigint**|Die verwendete Größe der betreffenden Datei. NULL, wenn die Zustandsspalte auf 5, 6 oder 7 festgelegt ist.<br /><br /> Bei Prüfpunktdateipaaren, die noch mit Daten aufgefüllt werden, wird diese Spalte nach dem nächsten Prüfpunkt aktualisiert.|  
|inserted_row_count|**bigint**|Die Anzahl der Zeilen in der Datendatei.|  
|deleted_row_count|**bigint**|Die Anzahl der gelöschten Zeilen in der Änderungsdatei.|  
|drop_table_deleted_row_count|**bigint**|Die Anzahl der Zeilen in den Datendateien, auf die sich die Tabellenlöschung auswirkt. Gilt für Datendateien, wenn die Zustandsspalte gleich 1 ist.<br /><br /> Zeigt die Anzahl der aus gelöschten Tabellen gelöschten Zeilen an. Statistiken drop_table_deleted_row_count werden kompiliert, nachdem die Arbeitsspeicher-Garbage Collection für die Zeilen aus gelöschten Tabellen abgeschlossen und ein Prüfpunkt erstellt wurde. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu starten, bevor die Statistiken zu gelöschten Tabellen in dieser Spalte angezeigt werden, werden sie im Rahmen der Wiederherstellung aktualisiert. Bei der Wiederherstellung werden keine Zeilen aus gelöschten Tabellen geladen. Statistiken zu gelöschten Tabellen werden während der Ladephase kompiliert und in dieser Spalte wiedergegeben, sobald die Wiederherstellung abgeschlossen ist.|  
|state|**int**|0-vorab erstellt<br /><br /> 1-Unterkonstruktion<br /><br /> 2 - ACTIVE<br /><br /> 3-Merge-Ziel<br /><br /> 4-zusammengeführte Quelle<br /><br /> 5: für Sicherung/hohe Verfügbarkeit erforderlich<br /><br /> 6-bei Übergang zu Tombstone<br /><br /> 7-Tombstone|  
|state_desc|**nvarchar(60)**|Precreated: ein kleiner Satz von Daten-und Änderungsdatei Paaren, die auch als Prüf Punkt Datei Paare (Cfps) bezeichnet werden, wird beibehalten, um alle Wartezeiten für die Zuordnung neuer Dateien während der Ausführung von Transaktionen zu minimieren oder auszuschließen. Dabei handelt es sich um Datendateien mit der vollständigen Größe von 128 MB und 8 MB große Änderungsdateien, die jedoch keine Daten enthalten. Die Anzahl der CFPs wird aus der Anzahl von logischen Prozessoren oder Zeitplanungsmodulen (einer pro Kern, kein Höchstwert) berechnet und beträgt mindestens 8. Dies ist ein fester Speichermehraufwand in Datenbanken mit speicheroptimierten Tabellen.<br /><br /> Unter dem Konstruktions Satz von Cfps, die seit dem letzten Prüfpunkt neu eingefügte und möglicherweise gelöschte Daten Zeilen speichern.<br /><br /> ACTIVE – Diese enthalten die eingefügten und gelöschten Zeilen aus den vorherigen geschlossenen Prüfpunkten. Diese CFPs enthalten alle erforderlichen eingefügten und gelöschten Zeilen, bevor der aktive Teil des Transaktionsprotokolls beim Neustart der Datenbank angewendet wird. Diese CFPs sind etwa doppelt so groß wie die In-Memory-Größe speicheroptimierter Tabellen, vorausgesetzt, die Zusammenführung hält mit der Transaktionsarbeitsauslastung mit.<br /><br /> Merge target: Das cFP speichert die konsolidierten Daten Zeilen aus den cFP (en), die durch die Zusammenschluss Richtlinie identifiziert wurden. Nachdem die Zusammenführung installiert wurde, befinden sich die MERGE TARGET-Übergänge im Status ACTIVE.<br /><br /> Zusammengeführte Quelle: Nachdem der Merge-Vorgang installiert wurde, werden die Quell-Cfps als zusammengeführte Quelle gekennzeichnet. Beachten Sie, dass die Auswertung der Zusammenführungsrichtlinie mehrere Zusammenführungen identifizieren kann, ein CFP jedoch nur an einem Zusammenführungsvorgang beteiligt sein darf.<br /><br /> Erforderlich für Sicherung/hohe Verfügbarkeit: Nachdem die Zusammenführung installiert wurde und das Merge-Ziel-cFP Teil eines permanenten Prüf Punkts ist, wechseln die CfPs der Merge-Quelle in diesen Zustand. CFPs in diesem Zustand werden benötigt, um die nahtlose Verwendung der Datenbank mit der speicheroptimierten Tabelle sicherzustellen.  Dies gilt beispielsweise für die Wiederherstellung von einem dauerhaften Prüfpunkt, um zu einem früheren Zustand zurückzukehren. Ein CFP kann für die Garbage Collection gekennzeichnet werden, sobald der Protokollkürzungspunkt hinter dem Transaktionsbereich liegt.<br /><br /> Bei der Umstellung auf Tombstone werden diese Cfps von der In-Memory OLTP-Engine nicht benötigt und können in die Garbage Collection aufgenommen werden. Dieser Status gibt an, dass diese CFPs warten, bis sie vom Hintergrundthread in den nächsten Zustand (d. h. TOMBSTONE) versetzt werden.<br /><br /> Tombstone: Diese Cfps warten auf eine Garbage Collection durch den FILESTREAM-Garbage Collector. ([sp_filestream_force_garbage_collection &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md))|  
|lower_bound_tsn|**bigint**|Die untere Grenze für Transaktionen, die in der Datei enthalten sind. NULL, wenn die Zustandsspalte ungleich 2, 3 oder 4 ist.|  
|upper_bound_tsn|**bigint**|Die obere Grenze für Transaktionen, die in der Datei enthalten sind. NULL, wenn die Zustandsspalte ungleich 2, 3 oder 4 ist.|  
|last_backup_page_count|**int**|Die logische Seitenanzahl, die bei der letzten Sicherung bestimmt wird. Diese gilt, wenn die Zustandsspalte auf 2, 3, 4 oder 5 festgelegt ist. NULL, wenn die Seitenanzahl unbekannt ist.|  
|delta_watermark_tsn|**int**|Die Transaktion des letzten Prüfpunkts, von dem in diese Änderungsdatei geschrieben wurde. Dies ist das Wasserzeichen der Änderungsdatei.|  
|last_checkpoint_recovery_lsn|**nvarchar (23)**|Die Wiederherstellungs-Protokollfolgenummer des letzten Prüfpunkts, von dem die Datei noch benötigt wird.|  
|tombstone_operation_lsn|**nvarchar (23)**|Die Datei wird gelöscht, sobald tombstone_operation_lsn hinter der Protokollfolgenummer des Protokollkürzungspunkts zurückfällt.|  
|logical_deletion_log_block_id|**bigint**|Gilt nur für Zustand 5.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die `VIEW DATABASE STATE`-Berechtigung auf dem Server.  
  
## <a name="use-cases"></a>Anwendungsfälle  
 Sie können den von In-Memory OLTP verwendeten Speicher wie folgt schätzen:  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
Führen Sie die folgende Abfrage aus, um eine Aufschlüsselung der Speicherauslastung nach Status und Dateityp anzuzeigen:
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
