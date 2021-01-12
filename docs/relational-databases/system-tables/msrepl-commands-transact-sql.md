---
description: MSrepl_commands (Transact-SQL)
title: MSrepl_commands (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSrepl_commands
- MSrepl_commands_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_commands system table
ms.assetid: 53b9f9cd-9429-47a0-aba2-908fc60e7036
author: cawrites
ms.author: chadam
ms.openlocfilehash: c939e1c5112aee07738ad5d368cb0ef05d804618
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091034"
---
# <a name="msrepl_commands-transact-sql"></a>MSrepl_commands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSrepl_commands** Tabelle enthält Zeilen mit replizierten Befehlen. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|Die ID der Verlegerdatenbank.|  
|**xact_seqno**|**varbinary(16)**|Die Transaktionssequenznummer.|  
|**type**|**int**|Der Befehlstyp.|  
|**article_id**|**int**|Die ID des Artikels.|  
|**originator_id**|**int**|Die ID des Urhebers.|  
|**command_id**|**int**|Die Befehls-ID.|  
|**partial_command**|**bit**|Gibt an, ob es sich um einen Teilbefehl handelt.|  
|**command**|**varbinary (1024)**|Der Befehlswert.|  
|**hashkey**|**int**|Nur intern verwendet.|  
|**originator_lsn**|**varbinary(16)**|Identifiziert die LSN des Befehls in der Ausgangsveröffentlichung. Wird in der Peer-zu-Peer-Transaktionsreplikation verwendet.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)  
  
  
