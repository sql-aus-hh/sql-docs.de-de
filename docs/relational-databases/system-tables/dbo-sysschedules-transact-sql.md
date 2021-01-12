---
description: dbo.sysschedules (Transact-SQL)
title: dbo.sysZeitpläne (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysschedules_TSQL
- sysschedules
- sysschedules_TSQL
- dbo.sysschedules
dev_langs:
- TSQL
helpviewer_keywords:
- sysschedules system table
ms.assetid: 4cac9237-7a69-4035-bb3e-928b76aad698
author: cawrites
ms.author: chadam
ms.openlocfilehash: 31c49370b1e2ed2b5685bf46b0918fd0173f7119
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096232"
---
# <a name="dbosysschedules-transact-sql"></a>dbo.sysschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält Informationen zu Auftragszeitplänen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|ID des Auftragszeitplans des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agents.|  
|**schedule_uid**|**uniqueidentifier**|Eindeutiger Bezeichner des Auftragszeitplans. Anhand dieses Wertes wird ein Zeitplan für verteilte Aufträge identifiziert.|  
|**originating_server_id**|**int**|ID des Masterservers, von dem der Auftragszeitplan stammt.|  
|**name**|**vom Datentyp sysname (nvarchar (128))**|Benutzerdefinierter Name für den Auftragszeitplan. Dieser Name muss innerhalb eines Auftrags eindeutig sein.|  
|**owner_sid**|**varbinary(85)**|Microsoft Windows *security_identifier* des Benutzers oder der Gruppe, der den Auftrags Zeitplan besitzt.|  
|**enabled**|**int**|Status des Auftragszeitplans:<br /><br /> **0** = nicht aktiviert.<br /><br /> **1** = aktiviert.<br /><br /> Wenn der Zeitplan nicht aktiviert ist, werden keine Aufträge nach dem Zeitplan ausgeführt.|  
|**freq_type**|**int**|Häufigkeit der Ausführung eines Auftrags für diesen Zeitplan.<br /><br /> **1** = nur einmal<br /><br /> **4** = täglich<br /><br /> **8** = wöchentlich<br /><br /> **16** = monatlich<br /><br /> **32** = monatlich, relativ zu **freq_interval**<br /><br /> **64** = wird ausgeführt, wenn der SQL Server-Agent Dienst gestartet wird.<br /><br /> **128** = wird ausgeführt, wenn sich der Computer im Leerlauf befindet|  
|**freq_interval**|**int**|Tage, an denen der Auftrag ausgeführt wird. Hängt vom Wert **freq_type** ab. Der Standardwert ist **0**(null) und gibt an, dass **freq_interval** nicht verwendet wird. In der folgenden Tabelle finden Sie die möglichen Werte und deren Auswirkungen.|  
|**freq_subday_type**|**int**|Einheiten für die **freq_subday_interval**. Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> <br /><br /> **1** : zum angegebenen Zeitpunkt<br /><br /> **2** : Sekunden<br /><br /> **4** : Minuten<br /><br /> **8** : Stunden|  
|**freq_subday_interval**|**int**|Anzahl der **freq_subday_type** Zeiträume zwischen den einzelnen Ausführungen des Auftrags.|  
|**freq_relative_interval**|**int**|Wenn **freq_interval** in jedem Monat auftritt, wenn **freq_type** **32** (monatlich, relativ) ist. Folgenden Werte sind möglich:<br /><br /> **0**  =  **freq_relative_interval** wird nicht verwendet.<br /><br /> **1** = zuerst<br /><br /> **2** = Sekunde<br /><br /> **4** = dritte<br /><br /> **8** = Fourth<br /><br /> **16** = zuletzt|  
|**freq_recurrence_**<br /><br /> **gebend**|**int**|Die Anzahl der Wochen oder Monate zwischen den geplanten Ausführungen eines Auftrags. **freq_recurrence_factor** wird nur verwendet,  wenn freq_type **8**, **16** oder **32** ist. Wenn diese Spalte den Wert **0** enthält, wird **freq_recurrence_factor** nicht verwendet.|  
|**active_start_date**|**int**|Datum, an dem die Ausführung eines Auftrags beginnen kann. Das Datum wird als YYYYMMDD formatiert. NULL steht für das Datum des heutigen Tages.|  
|**active_end_date**|**int**|Datum, an dem die Ausführung eines Auftrags enden kann. Für das Datum wird das Format YYYYMMDD verwendet.|  
|**active_start_time**|**int**|Uhrzeit an einem beliebigen Tag zwischen **active_start_date** und **active_end_date** die Ausführung des Auftrags beginnt. Die Zeit wird als HHMMSS im 24-Stunden-Format angegeben.|  
|**active_end_time**|**int**|Uhrzeit an einem beliebigen Tag zwischen **active_start_date** und **active_end_date** die Ausführung des Auftrags beendet wird. Die Zeit wird als HHMMSS im 24-Stunden-Format angegeben.|  
|**date_created**|**datetime**|Datum und Uhrzeit des Zeitpunkts, an dem der Zeitplan erstellt wurde.|  
|**date_modified**|**datetime**|Datum und Uhrzeit der letzten Änderung des Zeitplans.|  
|**version_number**|**int**|Aktuelle Versionsnummer des Zeitplans. Wenn ein Zeitplan z. b. zehnmal geändert wurde, ist die **version_number** 10.|  
  
|Wert von freq_type|Auswirkung auf freq_interval|  
|-------------------------|------------------------------|  
|**1** (einmal)|**freq_interval** nicht verwendet (**0**)|  
|**4** (täglich)|Alle **freq_interval** Tage|  
|**8** (wöchentlich)|**freq_interval** ist eine oder mehrere der folgenden:<br /><br /> **1** = Sonntag<br /><br /> **2** = Montag<br /><br /> **4** = Dienstag<br /><br /> **8** = Mittwoch<br /><br /> **16** = Donnerstag<br /><br /> **32** = Freitag<br /><br /> **64** = Samstag|  
|**16** (monatlich)|Am **freq_interval** Tag des Monats|  
|**32** (monatlich, relativ)|**freq_interval** ist einer der folgenden:<br /><br /> **1** = Sonntag<br /><br /> **2** = Montag<br /><br /> **3** = Dienstag<br /><br /> **4** = Mittwoch<br /><br /> **5** = Donnerstag<br /><br /> **6** = Freitag<br /><br /> **7** = Samstag<br /><br /> **8** = Tag<br /><br /> **9** = Wochentag<br /><br /> **10** = Wochenendtag|  
|**64** (wird gestartet, wenn SQL Server-Agent Dienst gestartet wird)|**freq_interval** nicht verwendet (**0**)|  
|**128** (wird ausgeführt, wenn der Computer im Leerlauf ist)|**freq_interval** nicht verwendet (**0**)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [dbo.sysjobzeitpläne &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
  
  
