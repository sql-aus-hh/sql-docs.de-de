---
description: MSdistribution_history (Transact-SQL)
title: MSdistribution_history (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdistribution_history
- MSdistribution_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_history system table
ms.assetid: 55665bd2-9e1d-4efc-8f60-c63a24f66b28
author: cawrites
ms.author: chadam
ms.openlocfilehash: 541a976e55a62dd9802f5d5a54cc31487ad5680e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091450"
---
# <a name="msdistribution_history-transact-sql"></a>MSdistribution_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSdistribution_history** Tabelle enthält Verlaufs Zeilen für die dem lokalen Verteiler zugeordneten Verteilungs-Agents. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
## <a name="definition"></a>Definition  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|Die ID des Verteilungs-Agents.|  
|**RunStatus**|**int**|Der Status wird ausgeführt:<br /><br /> **1** = Start.<br /><br /> **2** = erfolgreich.<br /><br /> **3** = wird ausgeführt.<br /><br /> **4** = im Leerlauf.<br /><br /> **5** = Wiederholungsversuch.<br /><br /> **6** = Fehler.|  
|**start_time**|**datetime**|Der Zeitpunkt, zu dem mit der Ausführung des Auftrags begonnen wird.|  
|**time**|**datetime**|Der Zeitpunkt der Protokollierung der Meldung.|  
|**duration**|**int**|Die Dauer der Meldungssitzung in Sekunden.|  
|**Kommentare**|**nvarchar(4000)**|Der Meldungstext.|  
|**xact_seqno**|**varbinary(16)**|Die Sequenznummer der zuletzt verarbeiteten Transaktion.|  
|**current_delivery_rate**|**float**|Die durchschnittliche Anzahl der Befehle, die seit dem letzten Verlaufseintrag pro Sekunde übermittelt wurden.|  
|**current_delivery_latency**|**int**|Die Latenzzeit zwischen dem Eintritt des Befehls in die Verteilungsdatenbank und seiner Anwendung auf den Abonnenten seit dem letzten Verlaufseintrag. In Millisekunden.|  
|**delivered_transactions**|**int**|Die Gesamtzahl der in der Sitzung übermittelten Transaktionen.|  
|**delivered_commands**|**int**|Die Gesamtzahl der in der Sitzung übermittelten Befehle.|  
|**average_commands**|**int**|Die durchschnittliche Anzahl der in der Sitzung übermittelten Befehle.|  
|**delivery_rate**|**float**|Die durchschnittliche Anzahl der pro Sekunde übermittelten Befehle.|  
|**delivery_latency**|**int**|Die Latenzzeit zwischen dem Eintritt des Befehls in die Verteilungsdatenbank und seiner Anwendung auf den Abonnenten. In Millisekunden.|  
|**total_delivered_commands**|**bigint**|Die Gesamtzahl der seit der Erstellung des Abonnements übermittelten Befehle.|  
|**error_id**|**int**|Die ID des Fehlers in der **MSrepl_error** -Systemtabelle.|  
|**updateable_row**|**bit**|Auf **1** festgelegt, wenn die Verlaufs Zeile überschrieben werden kann.|  
|**timestamp**|**timestamp**|Die Timestampspalte dieser Tabelle.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
