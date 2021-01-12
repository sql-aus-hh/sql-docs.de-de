---
description: dbo.sysjobsteps (Transact-SQL)
title: dbo.sysAuftrags Schritte (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobsteps
- dbo.sysjobsteps_TSQL
- sysjobsteps
- sysjobsteps_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobsteps system table
ms.assetid: 978b8205-535b-461c-91f3-af9b08eca467
author: cawrites
ms.author: chadam
ms.openlocfilehash: a3f59ef0d9a3e2a38a7834ed4395319ec935fc59
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102139"
---
# <a name="dbosysjobsteps-transact-sql"></a>dbo.sysjobsteps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält die Informationen für jeden Schritt in einem Auftrag, der vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent ausgeführt werden soll. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID des Auftrags.|  
|**step_id**|**int**|ID des Schritts im Auftrag.|  
|**step_name**|**sysname**|Der Name des Auftrags Schritts.|  
|**System**|**nvarchar(40)**|Name des vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent zur Ausführung des Auftragsschritts verwendeten Subsystems.|  
|**command**|**nvarchar(max)**|Der Befehl, der vom **Subsystem** ausgeführt werden soll.|  
|**flags**|**int**|Reserviert.|  
|**additional_parameters**|**ntext**|Reserviert.|  
|**cmdexec_success_code**|**int**|Der von **CmdExec** -Subsystem zurückgegebene Wert auf Fehlerebene, um den Erfolg anzugeben.|  
|**on_success_action**|**tinyint**|Aktion, die nach erfolgreicher Ausführung eines Schritts durchzuführen ist.<br /><br /> **1** = (Standard) beenden mit Erfolg<br /><br /> **2** = beenden mit Fehler<br /><br /> **3** = gehe zum nächsten Schritt<br /><br /> **4** = gehe zu Schritt _on_success_step_id_|
|**on_success_step_id**|**int**|ID des Schritts, der als Nächstes auszuführen ist, nachdem ein Schritt erfolgreich ausgeführt wurde.|  
|**on_fail_action**|**tinyint**|Aktion, die durchzuführen ist, wenn ein Schritt nicht erfolgreich ausgeführt wurde.<br /><br /> **1** = beenden mit Erfolg<br /><br /> **2** = (Standard) beenden mit Fehler<br /><br /> **3** = gehe zum nächsten Schritt<br /><br /> **4** = gehe zu Schritt _on_fail_step_id_|
|**on_fail_step_id**|**int**|ID des Schritts, der als Nächstes auszuführen ist, wenn ein Schritt nicht erfolgreich ausgeführt wurde.|  
|**server**|**sysname**|Reserviert.|  
|**database_name**|**sysname**|Der Name der Datenbank, in der der **Befehl** ausgeführt wird, wenn das **Subsystem** "zql" ist.|  
|**database_user_name**|**sysname**|Name des Datenbankbenutzers, dessen Konto verwendet wird, wenn der Schritt ausgeführt wird.|  
|**retry_attempts**|**int**|Anzahl der Wiederholungsversuche, wenn der Schritt einen Fehler erzeugt.|  
|**retry_interval**|**int**|Zeit, die zwischen Wiederholungsversuchen gewartet wird.|  
|**os_run_priority**|**int**|Reserviert.|  
|**output_file_name**|**nvarchar(200)**|Der Name der Datei, in der die Schritt Ausgabe gespeichert wird, wenn das **Subsystem** "TQL", "PowerShell" oder " **CmdExec**" ist _._|  
|**last_run_outcome**|**int**|Ergebnis der vorherigen Ausführung des Auftragsschritts.<br /><br /> **0** = fehlgeschlagen<br /><br /> **1** = erfolgreich<br /><br /> **2** = Wiederholung<br /><br /> **3** = abgebrochen<br /><br /> **5** = unbekannt|  
|**last_run_duration**|**int**|Dauer (hhmmss) der letzten Ausführung des Schritts.|  
|**last_run_retries**|**int**|Anzahl der Wiederholungsversuche bei der letzten Ausführung des Auftragsschritts.|  
|**last_run_date**|**int**|Datum (yyymmdd), an dem die Ausführung des Schritts zuletzt gestartet wurde.|  
|**last_run_time**|**int**|Uhrzeit (hhmmss), zu der die Ausführung des Schritts zuletzt gestartet wurde.|  
|**proxy_id**|**int**|Proxy für den Auftragsschritt.|  
|**step_uid**|**uniqueidentifier**|ID für den Auftragsschritt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server-Agent Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
