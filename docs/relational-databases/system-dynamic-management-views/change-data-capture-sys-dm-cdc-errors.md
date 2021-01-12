---
description: Change Data Capture-sys.dm_cdc_errors
title: sys.dm_cdc_errors (Transact-SQL) | Microsoft-Dokumentation
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
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ce058600c4a912e13695817a533f8e9ec4c8f856
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100123"
---
# <a name="change-data-capture---sysdm_cdc_errors"></a>Change Data Capture-sys.dm_cdc_errors
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt für jeden während der Change Data Capture-Protokollscansitzung gefundenen Fehler eine Zeile zurück.  
 
 
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID der Sitzung.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
|**phase_number**|**int**|Zahl, die die Phase angibt, in der die Sitzung zum Zeitpunkt des Fehlers aufgetreten ist. Eine Beschreibung der einzelnen Phasen finden Sie unter [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**entry_time**|**datetime**|Datum und Uhrzeit der Protokollierung des Fehlers. Dieser Wert entspricht dem Timestamp im SQL-Fehlerprotokoll.|  
|**error_number**|**int**|ID der Fehlermeldung.|  
|**error_severity**|**int**|Schweregrad des Fehlers, der zwischen 1 und 25 liegen kann.|  
|**error_state**|**int**|Zustandsnummer des Fehlers.|  
|**error_message**|**nvarchar(1024)**|Meldungstext des Fehlers.|  
|**start_lsn**|**nvarchar (23)**|LSN-Anfangswert der Zeilen, die beim Auftreten des Fehlers verarbeitet wurden.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
|**begin_lsn**|**nvarchar (23)**|LSN-Anfangswert der Transaktion, die beim Auftreten des Fehlers verarbeitet wurde.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
|**sequence_value**|**nvarchar (23)**|LSN-Wert der Zeilen, die beim Auftreten des Fehlers verarbeitet wurden.<br /><br /> 0 = Der Fehler trat nicht während einer Protokollscansitzung auf.|  
  
## <a name="remarks"></a>Bemerkungen  
 **sys.dm_cdc_errors** enthält Fehlerinformationen für die vorherigen 32-Sitzungen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung, um die **sys.dm_cdc_errors** dynamischen Verwaltungs Sicht abzufragen. Weitere Informationen zu Berechtigungen für dynamische Verwaltungs Sichten finden Sie unter [dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)   
 [sys.dm_repl_traninfo &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-repl-traninfo-transact-sql.md)  
  
  

