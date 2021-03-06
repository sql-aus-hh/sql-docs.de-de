---
title: Sys. dm_cdc_errors (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_cdc_errors_TSQL
- dm_cdc_errors
- sys.dm_cdc_errors
- sys.dm_cdc_errors_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cdc_errors dynamic management view
- change data capture [SQL Server], error reporting
ms.assetid: 898f2d76-9e63-45ef-94da-8034e86004ab
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ed8c72e0114804780cd3ee090b536eb28135e628
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47743268"
---
# <a name="change-data-capture---sysdmcdcerrors"></a>Change Data Capture - Sys. dm_cdc_errors
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Gibt für jeden während der Change Data Capture-Protokollscansitzung gefundenen Fehler eine Zeile zurück.  
 
 
|Spaltenname|Datentyp|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID der Sitzung.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
|**phase_number**|**int**|Anzahl, der angibt, die Phase, in die Sitzung in die zum Zeitpunkt der Fehler ist aufgetreten. Eine Beschreibung der einzelnen Phasen finden Sie unter [Sys. dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**entry_time**|**datetime**|Datum und Uhrzeit der Protokollierung des Fehlers. Dieser Wert entspricht dem Timestamp im SQL-Fehlerprotokoll.|  
|**error_number**|**int**|ID der Fehlermeldung.|  
|**error_severity**|**int**|Schweregrad des Fehlers, der zwischen 1 und 25 liegen kann.|  
|**error_state**|**int**|Zustandsnummer des Fehlers.|  
|**error_message**|**nvarchar(1024)**|Meldungstext des Fehlers.|  
|**start_lsn**|**nvarchar(23)**|LSN-Anfangswert der Zeilen, die beim Auftreten des Fehlers verarbeitet wurden.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
|**begin_lsn**|**nvarchar(23)**|LSN-Anfangswert der Transaktion, die beim Auftreten des Fehlers verarbeitet wurde.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
|**sequence_value**|**nvarchar(23)**|LSN-Wert der Zeilen, die beim Auftreten des Fehlers verarbeitet wurden.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
  
## <a name="remarks"></a>Hinweise  
 **Sys. dm_cdc_errors** enthält Fehlerinformationen für die vorherigen 32 Sitzungen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung zum Abfragen der **Sys. dm_cdc_errors** dynamische verwaltungssicht. Weitere Informationen zu Berechtigungen auf dynamische Verwaltungssichten, finden Sie unter [dynamische Verwaltungssichten und-Funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Siehe auch  
 [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)   
 [sys.dm_repl_traninfo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-repl-traninfo-transact-sql.md)  
  
  

