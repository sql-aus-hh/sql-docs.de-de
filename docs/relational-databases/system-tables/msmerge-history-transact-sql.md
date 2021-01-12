---
description: MSmerge_history (Transact-SQL)
title: MSmerge_history (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_history_TSQL
- MSmerge_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_history system table
ms.assetid: 936195ad-ca07-41a8-a1a0-6699b6e63403
author: cawrites
ms.author: chadam
ms.openlocfilehash: 060dc53fbce563ea7ae2552c50676739692ec750
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098175"
---
# <a name="msmerge_history-transact-sql"></a>MSmerge_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_history** Tabelle enthält Verlaufs Zeilen mit ausführlichen Beschreibungen der Ergebnisse früherer Merge-Agent Auftrags Sitzungen. Diese Tabelle enthält eine Zeile für jede Ausgabezeile der Momentaufnahme. Diese Tabelle wird in der Verteilungsdatenbank und in jeder Abonnementdatenbank verwendet. In der Verteilungsdatenbank enthält sie den Verlauf für alle Mergeveröffentlichungen und Abonnements, die den Verteiler verwenden. In den einzelnen Abonnementdatenbanken enthält sie den Verlauf für Veröffentlichungen, die der Abonnent abonniert hat.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Die ID des Merge-Agent-Auftrags|  
|**agent_id**|**int**|Die ID des Merge-Agents|  
|**Kommentare**|**nvarchar(255)**|Der Meldungstext.|  
|**error_id**|**int**|Die ID eines Fehlers in der [MSrepl_errors](../../relational-databases/system-tables/msrepl-errors-transact-sql.md) -Systemtabelle.|  
|**timestamp**|**timestamp**|Die Timestampspalte dieser Tabelle.|  
|**updatable_row**|**bit**|Auf **1** festgelegt, wenn die Verlaufs Zeile überschrieben werden kann.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
