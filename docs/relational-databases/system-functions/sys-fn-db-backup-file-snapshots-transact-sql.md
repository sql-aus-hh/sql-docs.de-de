---
description: sys.fn_db_backup_file_snapshots (Transact-SQL)
title: sys.fn_db_backup_file_snapshots (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 45010ff2-219f-4086-9ea4-016a6c17cddd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4976e0ae436fe1c9c59742b6580ae083b3e3c5c4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094937"
---
# <a name="sysfn_db_backup_file_snapshots-transact-sql"></a>sys.fn_db_backup_file_snapshots (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Gibt die den Datenbankdateien zugeordneten Azure-Momentaufnahmen zurück. Wenn die angegebene Datenbank nicht gefunden wird oder die Datenbankdateien nicht im Microsoft Azure BLOB Storage-Dienst gespeichert werden, werden keine Zeilen zurückgegeben. Verwenden Sie diese Systemfunktion zusammen mit der gespeicherten System Prozedur **sys.sp_delete_backup_file_snapshot** , um verwaiste Sicherungs Momentaufnahmen zu identifizieren und zu löschen. Weitere Informationen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_db_backup_file_snapshots   
   [ ( database_name ) ]  
```  
  
## <a name="arguments"></a>Argumente  
 *Database_name*  
 Der Name der Datenbank, die abgefragt wird. Wenn der Wert NULL ist, wird diese Funktion im aktuellen Daten Bankbereich ausgeführt.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|file_id|**int**|Die Datei-ID für die Datenbank. Lässt keine NULL-Werte zu.|  
|snapshot_time|**nvarchar(260)**|Der Zeitstempel der Momentaufnahme, wie er von der Rest-API zurückgegeben wird. Gibt NULL zurück, wenn keine Momentaufnahme vorhanden ist.|  
|snapshot_url|**nvarchar(360)**|Die vollständige URL der Datei Momentaufnahme. Gibt NULL zurück, wenn keine Momentaufnahme vorhanden ist.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung für die Datenbank.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_delete_backup_file_snapshot &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)   
 [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
  
  
