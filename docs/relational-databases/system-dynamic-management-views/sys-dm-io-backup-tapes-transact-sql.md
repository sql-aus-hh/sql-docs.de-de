---
description: sys.dm_io_backup_tapes (Transact-SQL)
title: sys.dm_io_backup_tapes (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_backup_tapes
- dm_io_backup_tapes_TSQL
- sys.dm_io_backup_tapes_TSQL
- dm_io_backup_tapes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_backup_tapes dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bc4eb0de019ee40145c15c344850744d9fef3b15
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101580"
---
# <a name="sysdm_io_backup_tapes-transact-sql"></a>sys.dm_io_backup_tapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt die Liste der Bandmedien und den Status von Einbindungsanforderungen für Sicherungen zurück.   
 
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**physical_device_name**|**nvarchar (520)**|Der Name des tatsächlichen physischen Mediums, auf dem eine Sicherung erstellt werden kann. Lässt keine NULL-Werte zu.|  
|**logical_device_name**|**nvarchar(256)**|Der vom Benutzer angegebene Name für das Laufwerk (von **sys.backup_devices**). NULL, wenn kein benutzerdefinierter Name verfügbar ist. Lässt NULL-Werte zu.|  
|**status**|**int**|Bandstatus:<br /><br /> 1 = Geöffnet und kann verwendet werden<br /><br /> 2 = Einbindung erfolgt<br /><br /> 3 = In Verwendung<br /><br /> 4 = Wird geladen<br /><br /> **Hinweis:** Wenn ein Band geladen wird (**Status = 4**), wird die Medien Bezeichnung noch nicht gelesen. Spalten, die Medien Bezeichnungs Werte (z. b. **media_sequence_number**) kopieren, zeigen erwartete Werte an, die sich von den tatsächlichen Werten auf dem Band unterscheiden können. Nachdem die Bezeichnung gelesen wurde, ändert sich der **Status** in " **3** " (in Verwendung), und die Medien Bezeichnungs Spalten spiegeln das eigentliche Band wider, das geladen wurde.<br /><br /> Lässt keine NULL-Werte zu.|  
|**status_desc**|**nvarchar (520)**|Bandstatusbeschreibung:<br /><br /> AVAILABLE<br /><br /> MOUNT PENDING<br /><br /> IN USE<br /><br /> LOADING MEDIA<br /><br /> Lässt keine NULL-Werte zu.|  
|**mount_request_time**|**datetime**|Uhrzeit, zu der die Einbindung angefordert wurde. NULL, wenn keine Einfügevorgang aussteht (**Status! = 2**). Lässt NULL-Werte zu.|  
|**mount_expiration_time**|**datetime**|Uhrzeit, zu der die Einbindungssanforderung abläuft (Timeout). NULL, wenn keine Einfügevorgang aussteht (**Status! = 2**). Lässt NULL-Werte zu.|  
|**database_name**|**nvarchar(256)**|Die Datenbank, die auf dem Medium gesichert werden soll. Lässt NULL-Werte zu.|  
|**spid**|**int**|Sitzungs-ID. Diese identifiziert den Benutzer des Bandes. Lässt NULL-Werte zu.|  
|**command**|**int**|Der Befehl, durch den die Sicherung ausgeführt wird. Lässt NULL-Werte zu.|  
|**command_desc**|**nvarchar(120)**|Beschreibung des Befehls. Lässt NULL-Werte zu.|  
|**media_family_id**|**int**|Index der Medien Familie (1...*n*) ist *n* die Anzahl der Medien Familien im Medien Satz. Lässt NULL-Werte zu.|  
|**media_set_name**|**nvarchar(256)**|Name des Mediensatzes (sofern vorhanden) gemäß Angabe durch die Option MEDIANAME zum Zeitpunkt der Erstellung des Mediensatzes. Lässt NULL-Werte zu.|  
|**media_set_guid**|**uniqueidentifier**|Bezeichner, der den Mediensatz eindeutig identifiziert. Lässt NULL-Werte zu.|  
|**media_sequence_number**|**int**|Index des Volumes innerhalb einer Medien Familie (1...*n*). Lässt NULL-Werte zu.|  
|**tape_operation**|**int**|Band Vorgang, der ausgeführt wird:<br /><br /> 1 = Lesen<br /><br /> 2 = Formatieren<br /><br /> 3 = Initialisieren<br /><br /> 4 = Anfügen<br /><br /> Lässt NULL-Werte zu.|  
|**tape_operation_desc**|**nvarchar(120)**|Ausgeführter Bandvorgang:<br /><br /> READ<br /><br /> FORMAT<br /><br /> INIT<br /><br /> APPEND<br /><br /> Lässt NULL-Werte zu.|  
|**mount_request_type**|**int**|Typ der Einbindungsanforderung:<br /><br /> 1 = Bestimmtes Band. Das von den Feldern **media_ \** _ identifizierte Band ist erforderlich.<br /><br /> 2 = Nächste Medienfamilie. Die nächste nicht wiederhergestellte Medienfamilie wird angefordert. Wird verwendet, wenn bei der Wiederherstellung weniger Medien als Medienfamilien vorhanden sind.<br /><br /> 3 = Anschlussband. Die Medienfamilie wird erweitert, und ein Anschlussband wird angefordert.<br /><br /> Lässt NULL-Werte zu.|  
|_ *mount_request_type_desc**|**nvarchar(120)**|Typ der Einbindungsanforderung:<br /><br /> SPECIFIC TAPE<br /><br /> NEXT MEDIA FAMILY<br /><br /> CONTINUATION VOLUME<br /><br /> Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer benötigt die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Mit e/a verbundene dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

