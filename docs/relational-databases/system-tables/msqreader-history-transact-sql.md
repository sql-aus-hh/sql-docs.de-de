---
description: MSqreader_history (Transact-SQL)
title: MSqreader_history (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSqreader_history
- MSqreader_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSqreader_history system table
ms.assetid: c5c91d39-513c-4a77-870b-c8ef74a1cd6b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 22053b5cae7f87c4cf2544ca0ed3ecc39dbda6a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100568"
---
# <a name="msqreader_history-transact-sql"></a>MSqreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSqreader_history** Tabelle enthält Verlaufs Zeilen für die dem lokalen Verteiler zugeordneten Warteschlangen Lese-Agents. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|Die ID des Warteschlangenlese-Agents.|  
|**publication_id**|**int**|Die ID der Veröffentlichung.|  
|**RunStatus**|**int**|Ausführungsstatus der Momentaufnahme:<br /><br /> **1** = Start.<br /><br /> **2** = erfolgreich.<br /><br /> **3** = wird ausgeführt.<br /><br /> **4** = im Leerlauf.<br /><br /> **5** = Wiederholungsversuch.<br /><br /> **6** = Fehler.|  
|**start_time**|**datetime**|Datum und Uhrzeit des Starts der Agentsitzung.|  
|**time**|**datetime**|Datum und Uhrzeit der zuletzt protokollierten Meldung.|  
|**duration**|**int**|Während der protokollierten Sitzungsaktivität verstrichene Zeit in Sekunden.|  
|**Kommentare**|**nvarchar(255)**|Beschreibender Text.|  
|**transaction_id**|**nvarchar(40)**|Mit der Meldung gespeicherte Transaktions-ID (sofern vorhanden).|  
|**transaction_status**|**int**|Status der Transaktion.|  
|**transactions_processed**|**int**|Kumulierte Anzahl der in der Sitzung verarbeiteten Transaktionen.|  
|**commands_processed**|**int**|Kumulierte Anzahl der in der Sitzung verarbeiteten Befehle.|  
|**delivery_rate**|**float(53)**|Die durchschnittliche Anzahl der pro Sekunde übermittelten Befehle.|  
|**transaction_rate**|**float(53)**|Rate der verarbeiteten Transaktionen.|  
|**Abonnenten**|**sysname**|Den Namen des Abonnenten.|  
|**SubscriberDB**|**sysname**|Der Name der Abonnement Datenbank.|  
|**error_id**|**int**|Wenn nicht 0 (null), stellt die Zahl eine [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fehlermeldung dar.|  
|**timestamp**|**timestamp**|Timestampspalte der Tabelle.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
