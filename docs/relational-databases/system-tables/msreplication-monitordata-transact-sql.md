---
description: MSreplication_monitordata (Transact-SQL)
title: MSreplication_monitordata (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_monitordata_TSQL
- MSreplication_monitordata
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_monitordata system table
ms.assetid: 843d3ffd-a1ef-4fd5-a744-c2252199793e
author: cawrites
ms.author: chadam
ms.openlocfilehash: bbe408296895f593de0af1da852b807082e8e7cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098160"
---
# <a name="msreplication_monitordata-transact-sql"></a>MSreplication_monitordata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSreplication_monitordata** Tabelle enthält zwischengespeicherte Daten, die vom Replikations Monitor verwendet werden, mit einer Zeile für jedes überwachte Abonnement. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**lastrefresh**|**datetime**|Datum und Uhrzeit des Updates der Überwachungsdaten|  
|**computetime**|**int**|Zeit (in Sekunden) zum Berechnen der Überwachungsdaten|  
|**publication_id**|**int**|Die Veröffentlichungs-ID.|  
|**publisher**|**sysname**|Der Name des Verlegers.|  
|**publisher_srvid**|**int**|Server-ID des Verlegers|  
|**publisher_db**|**sysname**|Der Name der Veröffentlichungs Datenbank.|  
|**ung**|**sysname**|Der Name der Veröffentlichung.|  
|**publication_type**|**int**|Der Veröffentlichungstyp. Die folgenden Werte sind möglich:<br /><br /> **0** = Transaktions Veröffentlichung<br /><br /> **1** = Momentaufnahme Veröffentlichung<br /><br /> **2** = Mergeveröffentlichung|  
|**agent_type**|**int**|Der Typ des Replikations-Agents. Die folgenden Werte sind möglich.<br /><br /> **1** = Momentaufnahmen-Agent<br /><br /> **2** = Protokolllese-Agent<br /><br /> **3** = Verteilungs-Agent<br /><br /> **4** = Merge-Agent<br /><br /> **9** = Warteschlangenlese-Agent|  
|**agent_id**|**int**|ID des Replikations-Agents|  
|**agent_name**|**sysname**|Name des Replikations-Agent-Auftrags|  
|**job_id**|**uniqueidentifier**|GUID des Replikations-Agent-Auftrags|  
|**status**|**int**|Der Status des Replikations-Agents. Die folgenden Werte sind möglich:<br /><br /> **1** = gestartet<br /><br /> **2** = erfolgreich<br /><br /> **3** = in Bearbeitung<br /><br /> **4** = im Leerlauf<br /><br /> **5** = wird wiederholt<br /><br /> **6** = fehlgeschlagen|  
|**isagentrunningnow**|**bit**|Ein Flag, das angibt, ob der Agentauftrag gerade ausgeführt wird, wobei der Wert **1** bedeutet, dass der Auftrag ausgeführt wird.|  
|**warning**|**int**|Schwellenwertwarnung, die von einem Abonnement generiert wird. Sie kann das Ergebnis des logischen OR von mindestens einem der folgenden Werte sein.<br /><br /> **1** = Ablauf: ein Abonnement für eine Transaktions Veröffentlichung hat die Beibehaltungs Dauer um mehr als den zulässigen Schwellenwert überschritten (als Prozentsatz der Beibehaltungs Dauer).<br /><br /> **2** = Latenz: die Zeit, die zum Replizieren von Daten von einem transaktionalen Verleger auf den Abonnenten benötigt wird, überschreitet den Schwellenwert (in Sekunden).<br /><br /> **4** = mergeablauf: ein Abonnement für eine Mergeveröffentlichung hat die Beibehaltungs Dauer um mehr als den zulässigen Schwellenwert überschritten, als Prozentsatz der Beibehaltungs Dauer. 8 = mergefastrunduration - die Zeit zum Synchronisieren eines Mergeabonnements über eine schnelle Netzwerkverbindung überschreitet den Schwellenwert (in Sekunden).<br /><br /> **16** = mergeslowrunduration: die Zeit für die Synchronisierung eines Mergeabonnements überschreitet den Schwellenwert über eine langsame oder DFÜ-Netzwerkverbindung (in Sekunden).<br /><br /> **32** = mergefastrauunspeed-die Übermittlungs Rate für Zeilen während der Synchronisierung eines Mergeabonnements konnte den Schwellenwert (in Zeilen pro Sekunde) über eine schnelle Netzwerkverbindung nicht aufrechterhalten.<br /><br /> **64** = mergeslowrunspeed-die Übermittlungs Rate für Zeilen während der Synchronisierung eines Mergeabonnements konnte den Schwellenwert (in Zeilen pro Sekunde) über eine langsame oder DFÜ-Netzwerkverbindung nicht aufrechterhalten.|  
|**last_distsync**|**datetime**|Datum und die Uhrzeit, wann der Verteilungs-Agent zuletzt ausgeführt wurde|  
|**agentstoptime**|**datetime**|Datum und die Uhrzeit der Beendigung der Momentaufnahme|  
|**distdb**|**sysname**|Name der Verteilungsdatenbank für das Abonnement|  
|**zurück**|**int**|Beibehaltungsdauer für die Veröffentlichung|  
|**time_stamp**|**datetime**|Nur intern verwendet.|  
|**worst_latency**|**int**|Die längste Latenzzeit (in Sekunden) für Datenänderungen, die vom Protokolllese-Agent oder vom Verteilungs-Agent für eine Transaktionsveröffentlichung weitergegeben werden.|  
|**best_latency**|**int**|Die kürzeste Latenzzeit (in Sekunden) für Datenänderungen, die vom Protokolllese-Agent oder vom Verteilungs-Agent für eine Transaktionsveröffentlichung weitergegeben werden.|  
|**avg_latency**|**int**|Die durchschnittliche Latenzzeit (in Sekunden) für Datenänderungen, die vom Protokolllese-Agent oder vom Verteilungs-Agent für eine Transaktionsveröffentlichung weitergegeben werden.|  
|**cur_latency**|**int**|Die Latenzzeit (in Sekunden) für Datenänderungen, die vom Protokolllese-Agent oder vom Verteilungs-Agent während der aktuellen Ausführung weitergegeben werden.|  
|**worst_runspeedPerf**|**int**|Die längste Synchronisierungszeit für die Mergeveröffentlichung|  
|**best_runspeedPerf**|**int**|Die kürzeste Synchronisierungszeit für die Mergeveröffentlichung|  
|**average_runspeedPerf**|**int**|Die durchschnittliche Synchronisierungszeit für die Mergeveröffentlichung|  
|**mergePerformance**|**int**|Die Leistung der letzten Synchronisierung im Vergleich zu allen Synchronisierungen des Abonnements. Sie ergibt sich aus der Übermittlungsrate der letzten Synchronisierung dividiert durch den Durchschnitt aller vorhergegangenen Übermittlungsraten.|  
|**mergelatestsessionrunduration**|**int**|Dauer der letzten Ausführung des Merge-Agents|  
|**mergelatestsessionrunspeed**|**float(53)**|Übermittlungsrate der letzten Ausführung des Merge-Agents|  
|**mergelatestsessionconnectiontype**|**int**|Die für die letzte Merge-Agent-Sitzung verwendete Verbindung. Die folgenden Werte sind möglich:<br /><br /> **1** = LAN (Local Area Network)<br /><br /> **2** = DFÜ-Netzwerkverbindung|  
|**retention_period_unit**|**tinyint**|Gibt die zum Definieren der Beibehaltungsdauer verwendete Einheit an. Die folgenden Werte sind möglich.<br /><br /> **1** = Woche<br /><br /> **2** = Monat<br /><br /> **3** = Jahr|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Programmgesteuerte Überwachen der Replikation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)   
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replmonitorhelpsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md)   
 [sp_replmonitorhelppublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md)   
 [sp_replmonitorhelppublisher &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md)   
 [sp_replmonitorhelpmergesession &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md)   
 [sp_replmonitorhelppublicationthresholds &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md)   
 [sp_replmonitorhelpmergesessiondetail &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md)  
  
  
