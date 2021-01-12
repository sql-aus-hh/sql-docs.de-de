---
description: sys.dm_filestream_non_transacted_handles (Transact-SQL)
title: sys.dm_filestream_non_transacted_handles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_filestream_non_transacted_handles_TSQL
- dm_filestream_non_transacted_handles
- dm_filestream_non_transacted_handles_TSQL
- sys.dm_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_non_transacted_handles dynamic management view
ms.assetid: 507ec125-67dc-450a-9081-94cde5444a92
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6b5082f5ad2105d91c23d3d1cca3e959b01343a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097640"
---
# <a name="sysdm_filestream_non_transacted_handles-transact-sql"></a>sys.dm_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt die derzeit geöffneten nicht transaktionalen Dateihandles an, die den FileTable-Daten zugeordnet sind.  
  
 Diese Sicht enthält eine Zeile pro geöffnetem Dateihandle. Da die Daten in dieser Sicht dem internen Livestatus des Servers entsprechen, ändern sich die Daten kontinuierlich mit dem Öffnen und Schließen der Handles. Diese Sicht enthält keine Verlaufsinformationen.  
  
 Weitere Informationen finden Sie unter [Verwalten von FileTables](../../relational-databases/blob/manage-filetables.md).  
  
|**Spalte**|**Typ**|**Beschreibung**|  
|----------------|--------------|---------------------|  
|database_id|INT|ID der Datenbank, die dem Handle zugeordnet ist.|  
|object_id|INT|Objekt-ID der FileTable, der das Handle zugeordnet ist.|  
|handle_id|INT|Eindeutiger Handlekontextbezeichner. Wird von der [sp_kill_filestream_non_transacted_handles &#40;gespeicherten Transact-&#41;SQL ](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md) -Prozedur verwendet, um ein bestimmtes Handle zu beenden.|  
|file_object_type|INT|Typ des Handles. Gibt die Ebene der Hierarchie an, für die das Handle geöffnet wurde, d. h. die Datenbank oder das Element.|  
|file_object_type_desc|nvarchar(120)|"Nicht definiert",<br />"SERVER_ROOT",<br />"DATABASE_ROOT",<br />"TABLE_ROOT",<br />"TABLE_ITEM"|  
|correlation_process_id|varbinary(8)|Enthält einen eindeutigen Bezeichner für den Prozess, von dem die Anforderung stammt.|  
|correlation_thread_id|varbinary(8)|Enthält einen eindeutigen Bezeichner für den Thread, von dem die Anforderung stammt.|  
|file_context|varbinary(8)|Zeiger auf das von diesem Handle verwendete Dateiobjekt.|  
|state|INT|Der aktuelle Status des Handles. Der Status kann aktiv, geschlossen oder abgebrochen sein.|  
|state_desc|nvarchar(120)|"Aktiv",<br />"Geschlossen",<br />Beendet|  
|current_workitem_type|INT|Der aktuelle Status für die Verarbeitung dieses Handles.|  
|current_workitem_type_desc|nvarchar(120)|"Nosetworkitemtype",<br />"Fftprekreateworkitem",<br />"Fftgetphysicaldateinameworkitem",<br />"Fftpostkreateworkitem",<br />"Fftprecleanupworkitem",<br />"Fftpostcleanupworkitem",<br />"Fftprecloseworkitem",<br />"Fftquerydirectoriyworkitem",<br />"Fftqueryinfoworkitem",<br />"Fftqueryvolumeinfoworkitem",<br />"Fftabtinfoworkitem",<br />"Fftschreitecompletionworkitem"|  
|fcb_id|BIGINT|FileTable-Dateikontrollblock-ID.|  
|item_id|varbinary (892)|Die Element-ID für eine Datei oder ein Verzeichnis. Ist möglicherweise NULL für Stammhandles des Servers.|  
|is_directory|bit|Dies ist ein Verzeichnis.|  
|item_name|nvarchar(512)|Name des Elements.|  
|opened_file_name|nvarchar(512)|Zu öffnender Pfad der ursprünglichen Anforderung.|  
|database_directory_name|nvarchar(512)|Teil des opened_file_name-Elements, das den Datenbankverzeichnisnamen darstellt.|  
|table_directory_name|nvarchar(512)|Teil des opened_file_name-Elements, das den Tabellenverzeichnisnamen darstellt.|  
|remaining_file_name|nvarchar(512)|Teil des opened_file_name-Elements, das den Namen des verbleibenden Verzeichnisses darstellt.|  
|open_time|datetime|Zeitpunkt, zu dem das Handle geöffnet wurde.|  
|flags|INT|ShareFlagsUpdatedToFcb = 0x1,<br />DeleteOnClose = 0x2,<br />NewFile = 0x4,<br />PostCreateDoneForNewFile = 0x8,<br />StreamFileOverwritten = 0x10,<br />RequestCancelled = 0x20,<br />NewFileCreationRolledBack = 0x40|  
|login_id|INT|ID des Prinzipals, der das Handle geöffnet hat.|  
|login_name|nvarchar(512)|Name des Prinzipals, der das Handle geöffnet hat.|  
|login_sid|varbinary(85)|SID des Prinzipals, der das Handle geöffnet hat.|  
|read_access|bit|Geöffnet für Lesezugriff.|  
|write_access|bit|Geöffnet für Schreibzugriff.|  
|delete_access|bit|Geöffnet für Löschzugriff.|  
|share_read|bit|Geöffnet mit share_read-Berechtigung.|  
|share_write|bit|Geöffnet mit share_write-Berechtigung.|  
|share_delete|bit|Geöffnet mit share_delete-Berechtigung.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten von FileTables](../../relational-databases/blob/manage-filetables.md)  
  
  
