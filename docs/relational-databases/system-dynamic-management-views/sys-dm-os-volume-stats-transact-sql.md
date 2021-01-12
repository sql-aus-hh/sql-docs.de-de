---
description: sys.dm_os_volume_stats (Transact-SQL)
title: sys.dm_os_volume_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_volume_stats_TSQL
- dm_os_volume_stats
- sys.dm_os_volume_stats
- sys.dm_os_volume_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_volume_stats dynamic management function
ms.assetid: fa1c58ad-8487-42ad-956c-983f2229025f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 32deca4f06709806d90ee7d25e25105f191ad23e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098820"
---
# <a name="sysdm_os_volume_stats-transact-sql"></a>sys.dm_os_volume_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-2008R2SP1-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-2008R2sp1-xxxx-xxxx-xxx-md.md)]

  Gibt Informationen zum Betriebssystemvolume (Verzeichnis) zurück, in dem die angegebenen Datenbanken und Dateien in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gespeichert sind. Verwenden Sie diese dynamische Verwaltungsfunktion, um die Attribute des physischen Datenträgers zu überprüfen oder um Informationen zum verfügbaren freien Speicherplatz für das Verzeichnis zurückzugeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sys.dm_os_volume_stats (database_id, file_id)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a>Argumente  
 *database_id*  
 Die ID der Datenbank. *database_id* ist vom Datentyp **int** und hat keinen Standardwert. Lässt keine NULL-Werte zu.  
  
 *file_id*  
 Die ID der Datei. *file_id* ist vom Datentyp **int** und hat keinen Standardwert. Lässt keine NULL-Werte zu.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
||||  
|-|-|-|  
|**Spalte**|**Datentyp**|**Beschreibung**|  
|**database_id**|**int**|Die ID der Datenbank. Darf nicht NULL sein.|  
|**file_id**|**int**|Die ID der Datei. Darf nicht NULL sein.|  
|**volume_mount_point**|**nvarchar(512)**|Der Einbindungspunkt, der das Stammverzeichnis des Volumes darstellt. Kann eine leere Zeichenfolge zurückgeben. Gibt NULL für das Linux-Betriebssystem zurück.|  
|**volume_id**|**nvarchar(512)**|Die ID des Betriebssystemvolumes. Kann eine leere Zeichenfolge zurückgeben. Gibt NULL für das Linux-Betriebssystem zurück.|  
|**logical_volume_name**|**nvarchar(512)**|Der Name des logischen Volumes. Kann eine leere Zeichenfolge zurückgeben. Gibt NULL für das Linux-Betriebssystem zurück.|  
|**file_system_type**|**nvarchar(512)**|Der Typ des Dateisystemvolumes (z. B. NTFS, FAT, RAW). Kann eine leere Zeichenfolge zurückgeben. Gibt NULL für das Linux-Betriebssystem zurück.|  
|**total_bytes**|**bigint**|Die Gesamtgröße des Volumes in Bytes. Darf nicht NULL sein.|  
|**available_bytes**|**bigint**|Der verfügbare freie Speicherplatz auf dem Volume. Darf nicht NULL sein.|  
|**supports_compression**|**tinyint**|Gibt an, ob das Volume eine Komprimierung durch das Betriebssystem unterstützt. Kann unter Windows nicht NULL sein und gibt bei Linux-Betriebssystem den Wert NULL zurück.|  
|**supports_alternate_streams**|**tinyint**|Gibt an, ob das Volume alternative Datenströme unterstützt. Kann unter Windows nicht NULL sein und gibt bei Linux-Betriebssystem den Wert NULL zurück.|  
|**supports_sparse_files**|**tinyint**|Gibt an, ob das Volume Sparsedateien unterstützt.  Kann unter Windows nicht NULL sein und gibt bei Linux-Betriebssystem den Wert NULL zurück.|  
|**is_read_only**|**tinyint**|Gibt an, ob das Volume derzeit als schreibgeschützt gekennzeichnet ist. Darf nicht NULL sein.|  
|**is_compressed**|**tinyint**|Gibt an, ob dieses Volume derzeit komprimiert ist. Kann unter Windows nicht NULL sein und gibt bei Linux-Betriebssystem den Wert NULL zurück.|  
|**incurs_seek_penalty**|**tinyint**|Gibt den Typ des Speichers an, der dieses Volume unterstützt. Mögliche Werte:<br /><br />0: keine Such Strafe auf diesem Volume, normalerweise wenn das Speichergerät PMM oder SSD ist<br /><br />1: Durchsuchen des Volumes auf diesem Volume (in der Regel, wenn das Speichergerät HDD ist)<br /><br />2: der Speichertyp kann nicht ermittelt werden, wenn sich das Volume in einem UNC-Pfad oder in bereitgestellten Freigaben befindet.<br /><br />NULL: der Speichertyp kann unter dem Linux-Betriebssystem nicht bestimmt werden.<br /><br />**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (beginnend mit [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] )|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die `VIEW SERVER STATE`-Berechtigung.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-return-total-space-and-available-space-for-all-database-files"></a>A. Zurückgeben des gesamten und des verfügbaren freien Speicherplatzes für alle Datenbankdateien  
 Im folgenden Beispiel werden der gesamte Speicherplatz und der verfügbare freie Speicherplatz (in Bytes) für alle Datenbankdateien in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zurückgegeben.  
  
```sql  
SELECT f.database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.master_files AS f  
CROSS APPLY sys.dm_os_volume_stats(f.database_id, f.file_id);  
```  
  
### <a name="b-return-total-space-and-available-space-for-the-current-database"></a>B. Zurückgeben des gesamten und des verfügbaren freien Speicherplatzes für die aktuelle Datenbank  
 Im folgenden Beispiel werden der gesamte Speicherplatz und der verfügbare freie Speicherplatz (in Bytes) für die Datenbankdateien der aktuellen Datenbank zurückgegeben.  
  
```sql  
SELECT database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.database_files AS f  
CROSS APPLY sys.dm_os_volume_stats(DB_ID(f.name), f.file_id);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.master_files &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  
