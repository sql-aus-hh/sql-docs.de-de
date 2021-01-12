---
description: Change Data Capture-sys.dm_cdc_log_scan_sessions
title: sys.dm_cdc_log_scan_sessions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_cdc_log_scan_sessions
- dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], log scan reporting
- sys.dm_cdc_log_scan_sessions dynamic management view
ms.assetid: d337e9d0-78b1-4a07-8820-2027d0b9f87c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 312a0577d6ac2d092b01b34fcdb2aa034d36995d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100069"
---
# <a name="change-data-capture---sysdm_cdc_log_scan_sessions"></a>Change Data Capture-sys.dm_cdc_log_scan_sessions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Protokollscansitzung in der aktuellen Datenbank zurück. Die letzte zurückgegebene Zeile stellt die aktuelle Sitzung dar. Mithilfe dieser Sicht können Sie Statusinformationen zur aktuellen Protokollscansitzung oder aggregierte Informationen zu allen Sitzungen zurückzugeben, seit die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das letzte Mal gestartet wurde.  
   
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID der Sitzung.<br /><br /> 0 = Die in dieser Zeile zurückgegebenen Daten sind ein Aggregat aller Sitzungen, seit die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das letzte Mal gestartet wurde.|  
|**start_time**|**datetime**|Zeitpunkt, zu dem die Sitzung begonnen wurde.<br /><br /> Wenn **session_id** = 0, wird die Zeit für die aggregierte Datensammlung gestartet.|  
|**end_time**|**datetime**|Zeitpunkt, zu dem die Sitzung beendet wurde.<br /><br /> NULL = Sitzung ist aktiv.<br /><br /> Wenn **session_id** = 0, der Zeitpunkt, zu dem die letzte Sitzung beendet wurde.|  
|**duration**|**bigint**|Die Dauer (in Sekunden) der Sitzung.<br /><br /> 0 = Die Sitzung enthält keine Change Data Capture-Transaktionen.<br /><br /> Wenn **session_id** = 0, die Summe der Dauer (in Sekunden) aller Sitzungen mit Change Data Capture Transaktionen.|  
|**scan_phase**|**nvarchar(200)**|Die aktuelle Phase der Sitzung. Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt:<br /><br /> 1: Lese Konfiguration<br />2: erste Überprüfung, Aufbau einer Hash Tabelle<br />3: zweite Überprüfung<br />4: zweite Überprüfung<br />5: zweite Überprüfung<br />6: Schema Versionsverwaltung<br />7: Letzte Überprüfung<br />8: abgeschlossen<br /><br /> Wenn **session_id** = 0 ist, ist dieser Wert immer "Aggregate".|  
|**error_count**|**int**|Anzahl der aufgetretenen Fehler.<br /><br /> Wenn **session_id** = 0, die Gesamtanzahl der Fehler in allen Sitzungen.|  
|**start_lsn**|**nvarchar (23)**|Start-LSN für die Sitzung.<br /><br /> Wenn **session_id** = 0, die Start-LSN für die letzte Sitzung.|  
|**current_lsn**|**nvarchar (23)**|Aktuelle LSN, die gescannt wird.<br /><br /> Wenn **session_id** = 0 ist, ist die aktuelle LSN 0.|  
|**end_lsn**|**nvarchar (23)**|Letzte LSN für die Sitzung.<br /><br /> NULL = Sitzung ist aktiv.<br /><br /> Wenn **session_id** = 0, die Ende-LSN für die letzte Sitzung.|  
|**tran_count**|**bigint**|Anzahl der verarbeiteten Change Data Capture-Transaktionen. Dieser Wert wird in Phase 2 aufgefüllt.<br /><br /> Wenn **session_id** = 0, die Anzahl der verarbeiteten Transaktionen in allen Sitzungen.|  
|**last_commit_lsn**|**nvarchar (23)**|LSN des letzten verarbeiteten Protokolldatensatzes für den Commit.<br /><br /> Wenn **session_id** = 0, die LSN des letzten Protokolldaten Satzes für den Commit für eine beliebige Sitzung.|  
|**last_commit_time**|**datetime**|Zeitpunkt, zu dem der letzte Protokolldatensatz für den Commit verarbeitet wurde.<br /><br /> Wenn **session_id** = 0, der Zeitpunkt des letzten Commit-Protokolldaten Satzes für eine beliebige Sitzung.|  
|**log_record_count**|**bigint**|Anzahl der gescannten Protokolldatensätze.<br /><br /> Wenn **session_id** = 0, die Anzahl der Datensätze, die für alle Sitzungen gescannt werden.|  
|**schema_change_count**|**int**|Anzahl der erkannten Vorgänge in der Datendefinitionssprache (Data Definition Language, DDL). Dieser Leistungsindikator wird in Phase 6 aufgefüllt.<br /><br /> Wenn **session_id** = 0, die Anzahl der in allen Sitzungen verarbeiteten DDL-Vorgänge.|  
|**command_count**|**bigint**|Anzahl der verarbeiteten Befehle.<br /><br /> Wenn **session_id** = 0, die Anzahl der in allen Sitzungen verarbeiteten Befehle.|  
|**first_begin_cdc_lsn**|**nvarchar (23)**|Erste LSN, die Change Data Capture-Transaktionen enthalten hat.<br /><br /> Wenn **session_id** = 0, die erste LSN, die Change Data Capture Transaktionen enthielt.|  
|**last_commit_cdc_lsn**|**nvarchar (23)**|LSN des letzten Protokolldatensatzes für den Commit, der Change Data Capture-Transaktionen enthalten hat.<br /><br /> Wenn **session_id** = 0, die LSN des letzten Protokolldaten Satzes für den Commit für eine beliebige Sitzung, die Change Data Capture Transaktionen enthielt|  
|**last_commit_cdc_time**|**datetime**|Zeitpunkt, zu dem der letzte Protokolldatensatz für den Commit verarbeitet wurde, der Change Data Capture-Transaktionen enthalten hat.<br /><br /> Wenn **session_id** = 0, der Zeitpunkt des letzten Commit-Protokolldaten Satzes für jede Sitzung, die Change Data Capture Transaktionen enthielt.|  
|**Schleifen**|**int**|Der Unterschied in Sekunden zwischen **end_time** und **last_commit_cdc_time** in der Sitzung. Dieser Leistungsindikator wird am Ende der Phase 7 aufgefüllt.<br /><br /> Wenn **session_id** = 0, der letzte Latenz Wert, der nicht NULL ist, wird von einer Sitzung aufgezeichnet.|  
|**empty_scan_count**|**int**|Anzahl der aufeinander folgenden Sitzungen, die keine Change Data Capture-Transaktionen enthalten haben.|  
|**failed_sessions_count**|**int**|Anzahl der fehlgeschlagenen Sitzungen.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Werte in dieser dynamischen Verwaltungssicht werden immer dann zurückgesetzt, wenn die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartet wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung, um die **sys.dm_cdc_log_scan_sessions** dynamischen Verwaltungs Sicht abzufragen. Weitere Informationen zu Berechtigungen für dynamische Verwaltungs Sichten finden Sie unter [dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zur aktuellen Sitzung zurückgegeben.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT session_id, start_time, end_time, duration, scan_phase,  
    error_count, start_lsn, current_lsn, end_lsn, tran_count,  
    last_commit_lsn, last_commit_time, log_record_count, schema_change_count,  
    command_count, first_begin_cdc_lsn, last_commit_cdc_lsn,   
    last_commit_cdc_time, latency, empty_scan_count, failed_sessions_count  
FROM sys.dm_cdc_log_scan_sessions  
WHERE session_id = (SELECT MAX(b.session_id) FROM sys.dm_cdc_log_scan_sessions AS b);  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_cdc_errors &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)  
  
  

