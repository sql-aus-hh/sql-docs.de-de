---
title: Backup Mediaset (Transact-SQL) | Microsoft-Dokumentation
description: Verweis für backupmediaset, das eine Zeile für jeden Sicherungsmedien Satz enthält.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupmediaset
- backupmediaset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], backupmediaset system table
- backupmediaset system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: 10b4d744e7ef4e0d11a9788580ea7f8c5a67bd1f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091578"
---
# <a name="backupmediaset-transact-sql"></a>backupmediaset (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Enthält eine Zeile für jeden Sicherungsmediensatz. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Eindeutige Medien Satz-Identifikationsnummer. Identität, Primärschlüssel.|  
|**media_uuid**|**uniqueidentifier**|UUID des Mediensatzes. Alle [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Mediensätze verfügen über eine UUID.<br /><br /> Bei früheren Versionen von ist [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die **media_uuid** Spalte jedoch möglicherweise NULL (**media_family_count** ist 1), wenn ein Medien Satz nur eine Medien Familie enthält.|  
|**media_family_count**|**tinyint**|Anzahl der Medienfamilien im Mediensatz. Kann den Wert NULL haben.|  
|**name**|**nvarchar(128)**|Name des Mediensatzes. Kann den Wert NULL haben.<br /><br /> Weitere Informationen finden Sie unter "MEDIANAME" und "mediadeabo" in [Backup &#40;Transact-SQL-&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**description**|**nvarchar(255)**|Textbeschreibung des Mediensatzes. Kann den Wert NULL haben.<br /><br /> Weitere Informationen finden Sie unter "MEDIANAME" und "mediadeabo" in [Backup &#40;Transact-SQL-&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**software_name**|**nvarchar(128)**|Name der Sicherungssoftware, mit der die Medienbezeichnung geschrieben wurde. Kann den Wert NULL haben.|  
|**software_vendor_id**|**int**|ID des Softwareanbieters, der die Sicherungsmedienbezeichnung geschrieben hat. Kann den Wert NULL haben.<br /><br /> Der Wert für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist hexadezimal 0x1200.|  
|**MTF_major_version**|**tinyint**|Hauptversionsnummer von MTF ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format), das zum Generieren dieses Mediensatzes verwendet wurde. Kann den Wert NULL haben.|  
|**mirror_count**|**tinyint**|Anzahl der Spiegel im Mediensatz.|  
|**is_password_protected**|**bit**|Gibt an, ob der Mediensatz kennwortgeschützt ist:<br /><br /> 0 = Nicht geschützt<br /><br /> 1 = Geschützt|  
|**is_compressed**|**bit**|Gibt an, ob die Sicherung komprimiert ist:<br /><br /> 0 = nicht komprimiert<br /><br /> 1 = komprimiert<br /><br /> Während eines **msdb** -Upgrades wird dieser Wert auf NULL festgelegt. Dies gibt eine nicht komprimierte Sicherung an.|  
|**is_encrypted**|**Trate**|Gibt an, ob die Sicherung verschlüsselt ist:<br /><br /> 0 = Nicht verschlüsselt<br /><br /> 1 = Verschlüsselt.|  
  
## <a name="remarks"></a>Bemerkungen  
 RESTORE VERIFYONLY FROM *backup_device* mit LOADHISTORY füllt die Spalten der **Backup Mediaset** -Tabelle mit den entsprechenden Werten aus dem Medien Satz Header auf.  
  
 Führen Sie die gespeicherte Prozedur [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) aus, um die Anzahl der Zeilen in dieser Tabelle und in anderen Sicherungs-und Verlaufs Tabellen zu verringern.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sichern und Wiederherstellen von Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Systemtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
