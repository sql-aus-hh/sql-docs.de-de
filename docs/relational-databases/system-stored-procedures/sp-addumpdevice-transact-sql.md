---
description: sp_addumpdevice (Transact-SQL)
title: sp_addumpdevice (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_addumpdevice_TSQL
- sp_addumpdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], defining
- sp_addumpdevice
ms.assetid: c2d2ae49-0808-46d8-8444-db69a69d0ec3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0a05127f4f6ddcd70fbb00cc5ae3bd2d22fe152
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813310"
---
# <a name="sp_addumpdevice-transact-sql"></a>sp_addumpdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level)).  

Fügt einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Sicherungsmedium hinzu.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_addumpdevice [ @devtype = ] 'device_type'   
    , [ @logicalname = ] 'logical_name'   
    , [ @physicalname = ] 'physical_name'  
      [ , { [ @cntrltype = ] controller_type |  
          [ @devstatus = ] 'device_status' }  
      ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @devtype = ] 'device_type'` Ist der Typ des Sicherungs Mediums. *device_type* ist vom Datentyp **varchar (20)** und hat keinen Standardwert. die folgenden Werte sind möglich:  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|**Diskette**|Festplattendatei als Sicherungsmedium.|  
|**Bänder**|Alle von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows unterstützten Bandmedien.<br /><br /> Hinweis: Die Unterstützung für Bandsicherungsgeräte wird in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.|  
  
`[ @logicalname = ] 'logical_name'` Der logische Name des Sicherungs Mediums, das in den Backup-und RESTORE-Anweisungen verwendet wird. *logical_name* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert und darf nicht NULL sein.  
  
`[ @physicalname = ] 'physical_name'` Der physische Name des Sicherungs Mediums. Physische Namen müssen den Regeln für Dateinamen des Betriebssystems oder den UNC-Konventionen für Netzwerkgeräte entsprechen und einen vollständigen Pfad angeben. *physical_name* ist vom Datentyp **nvarchar (260)** und hat keinen Standardwert und darf nicht NULL sein.  
  
 Beim Erstellen eines Sicherungsmediums auf einem Remotenetzwerk-Speicherort sollten Sie sicherstellen, dass das Benutzerkonto, unter dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] gestartet wurde, über entsprechende Schreibberechtigungen auf dem Remotecomputer verfügt.  
  
 Wenn Sie ein Bandgerät hinzufügen, muss dieser Parameter der physische Name sein, der von Windows dem lokalen Bandgerät zugewiesen wird. Beispiel: **\\ \\ .\TAPE0** für das erste Bandgerät auf dem Computer. Das Bandmedium muss an den Server angefügt werden; eine Remoteverwendung ist nicht möglich. Schließen Sie Namen, die andere als alphanumerische Zeichen enthalten, in Anführungszeichen ein.  
  
> [!NOTE]  
>  Diese Prozedur gibt den angegebenen physischen Namen in den Katalog ein. Die Prozedur nimmt jedoch keine Erstellung des Mediums und keinen Zugriff auf das Medium vor.  
  
`[ @cntrltype = ] 'controller_type'` Veralteten. Dieser Parameter wird ignoriert, wenn er angegeben wird. Er wird lediglich aus Gründen der Abwärtskompatibilität unterstützt. Die neue Verwendung von **sp_addumpdevice** sollte diesen Parameter weglassen.  
  
`[ @devstatus = ] 'device_status'` Veralteten. Dieser Parameter wird ignoriert, wenn er angegeben wird. Er wird lediglich aus Gründen der Abwärtskompatibilität unterstützt. Die neue Verwendung von **sp_addumpdevice** sollte diesen Parameter weglassen.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_addumpdevice** fügt der **sys.backup_devices** -Katalog Sicht ein Sicherungsmedium hinzu. Auf das Gerät kann dann in BACKUP- und RESTORE-Anweisungen logisch verwiesen werden. **sp_addumpdevice** führt keinen Zugriff auf das physische Gerät aus. Ein Zugriff auf das angegebene Medium erfolgt nur, wenn eine BACKUP- oder RESTORE-Anweisung ausgeführt wird. Das Erstellen eines logischen Sicherungsmediums kann BACKUP- und RESTORE-Anweisungen vereinfachen, wo das Angeben des Gerätenamens eine Alternative zu einer "TAPE ="- oder "DISK ="-Klausel zum Angeben des Pfades darstellt.  
  
 Besitz- und Berechtigungsprobleme können das Verwenden von Datenträger- oder Dateisicherungsmedien beeinträchtigen. Stellen Sie sicher, dass für das Windows-Konto, unter dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] gestartet wurde, entsprechende Dateiberechtigungen angegeben sind.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] unterstützt Sicherungen auf den Bandmedien, die auch von Windows unterstützt werden. Weitere Informationen zu Bandmedien, die von Windows unterstützt werden, finden Sie in der Hardwarekompatibilitätsliste für Windows. Verwenden Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], um die auf dem Computer verfügbaren Bandmedien anzuzeigen.  
  
 Verwenden Sie nur die empfohlenen Bänder für die jeweiligen Bandlaufwerke entsprechend den Angaben der Laufwerkhersteller. Bei DAT-Laufwerken (Digital Audio Tape) sollten Sie nur DAT-Bänder verwenden, die den Bandvorschriften für die Computerverwendung entsprechen (DDS, Digital Data Storage).  
  
 **sp_addumpdevice** kann nicht innerhalb einer Transaktion ausgeführt werden.  
  
 Um ein Gerät zu löschen, verwenden Sie [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) oder[SQL Server Management Studio](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **diskadmin** .  
  
 Erfordert die Berechtigung zum Schreiben auf den Datenträger.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-adding-a-disk-dump-device"></a>A. Hinzufügen eines Datenträgersicherungsmediums  
 Im folgenden Beispiel wird das Datenträgersicherungsmedium `mydiskdump` mit dem physischen Namen `c:\dump\dump1.bak` hinzugefügt.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak';  
```  
  
### <a name="b-adding-a-network-disk-backup-device"></a>B. Hinzufügen eines Netzwerk-Datenträgersicherungsmediums  
 Im folgenden Beispiel wird ein Remote-Datenträgersicherungsmedium mit dem Namen `networkdevice` dargestellt. Das Benutzerkonto, unter dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] gestartet wurde, muss für diese Remotedatei (`\\<servername>\<sharename>\<path>\<filename>.bak`) über entsprechende Berechtigungen verfügen.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'networkdevice',  
    '\\<servername>\<sharename>\<path>\<filename>.bak';  
```  
  
### <a name="c-adding-a-tape-backup-device"></a>C. Hinzufügen eines Bandsicherungsmediums  
 Im folgenden Beispiel wird das Gerät `tapedump1` mit dem physischen Namen `\\.\tape0` hinzugefügt.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'tape', 'tapedump1', '\\.\tape0';  
```  
  
### <a name="d-backing-up-to-a-logical-backup-device"></a>D: Sichern auf ein logisches Sicherungsmedium  
 Im folgenden Beispiel wird ein logisches Sicherungsmedium, `AdvWorksData`, für eine Sicherungs-Datenträgerdatei erstellt. Im Beispiel wird dann die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf diesem logischen Sicherungsmedium gesichert.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\AdvWorksData.bak';  
GO  
BACKUP DATABASE AdventureWorks2012   
 TO AdvWorksData  
   WITH FORMAT;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherungsmedien &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Definieren eines logischen Sicherungsmediums für eine Datenträgerdatei &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Definieren eines logischen Sicherungsmediums für ein Bandlaufwerk &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
