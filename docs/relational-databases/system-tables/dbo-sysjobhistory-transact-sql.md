---
description: dbo.sysjobhistory (Transact-SQL)
title: dbo.sysJobHistory (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/24/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobhistory_TSQL
- dbo.sysjobhistory
- sysjobhistory
- sysjobhistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobhistory system table
ms.assetid: 1b1fcdbb-2af2-45e6-bf3f-e8279432ce13
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6fa83e3df6cf6f05420341c497c8b3bfd2a481df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098722"
---
# <a name="dbosysjobhistory-transact-sql"></a>dbo.sysjobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Enthält Informationen zur Ausführung geplanter Aufträge durch den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent.
  
> [!NOTE]
> In den meisten Fällen werden die Daten erst aktualisiert, nachdem der Auftrags Schritt abgeschlossen wurde, und die Tabelle enthält in der Regel keine Datensätze für Auftrags Schritte, die derzeit ausgeführt werden. in einigen Fällen enthalten die zugrunde *liegenden Prozesse jedoch* Informationen zu laufenden Auftrags Schritten.

Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Eindeutiger Bezeichner für die Zeile.|  
|**job_id**|**uniqueidentifier**|Auftrags-ID.|  
|**step_id**|**int**|ID des Schritts im Auftrag.|  
|**step_name**|**sysname**|Name des Schritts|  
|**sql_message_id**|**int**|ID einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlermeldung, die beim Fehlschlagen des Auftrags zurückgegeben wird.|  
|**sql_severity**|**int**|Schweregrad eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlers.|  
|**Nachricht**|**nvarchar(4000)**|Text (falls vorhanden) eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlers.|  
|**run_status**|**int**|Status der Auftragsausführung:<br /><br /> **0** = fehlgeschlagen<br /><br /> **1** = erfolgreich<br /><br /> **2** = Wiederholung<br /><br /> **3** = abgebrochen<br /><br />**4** = in Bearbeitung|  
|**run_date**|**int**|Datum, an dem die Ausführung des Auftrags oder Schrittes gestartet wurde. Bei einem In-Progress-Verlauf ist dies das Datum/die Uhrzeit, an dem bzw. zu der der Verlauf geschrieben wurde.|  
|**run_time**|**int**|Zeitpunkt, zu dem der Auftrag oder Schritt im **HHMMSS** -Format gestartet wurde.|  
|**run_duration**|**int**|Verstrichene Zeit für die Ausführung des Auftrags oder Schritts im **HHMMSS** -Format.|  
|**operator_id_emailed**|**int**|ID des Operators, der bei Abschluss des Auftrags benachrichtigt wurde.|  
|**operator_id_netsent**|**int**|ID des Operators, der bei Abschluss des Auftrags durch eine Meldung benachrichtigt wurde.|  
|**operator_id_paged**|**int**|ID des Operators, der mithilfe eines Pagers bei Abschluss des Auftrags benachrichtigt wurde.|  
|**retries_attempted**|**int**|Anzahl der Wiederholungsversuche für den Auftrag oder Schritt.|  
|**server**|**sysname**|Name des Servers, auf dem der Auftrag ausgeführt wurde.|  
  
  ## <a name="example"></a>Beispiel
 Mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] Abfrage werden die **run_time** -und **run_duration** Spalten in ein benutzerfreundliches Format konvertiert.  Führen Sie das Skript in aus [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .
 
 ```sql
 SET NOCOUNT ON;
 
 SELECT sj.name,
        sh.run_date,
        sh.step_name,
        STUFF(STUFF(RIGHT(REPLICATE('0', 6) +  CAST(sh.run_time as varchar(6)), 6), 3, 0, ':'), 6, 0, ':') 'run_time',
        STUFF(STUFF(STUFF(RIGHT(REPLICATE('0', 8) + CAST(sh.run_duration as varchar(8)), 8), 3, 0, ':'), 6, 0, ':'), 9, 0, ':') 'run_duration (DD:HH:MM:SS)  '
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobhistory sh
ON sj.job_id = sh.job_id
```
