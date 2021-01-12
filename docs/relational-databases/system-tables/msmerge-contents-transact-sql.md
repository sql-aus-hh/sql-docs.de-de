---
description: MSmerge_contents (Transact-SQL)
title: MSmerge_contents (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_contents
- MSmerge_contents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_contents system table
ms.assetid: 8d68a61a-683f-4b20-92f9-c0a8d9ba0ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 52edaacaa839beeedb60b706803d268528d98492
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091431"
---
# <a name="msmerge_contents-transact-sql"></a>MSmerge_contents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_contents** Tabelle enthält eine Zeile für jede Zeile, die seit der Veröffentlichung in der aktuellen Datenbank geändert wurde. Diese Tabelle wird vom Mergeprozess verwendet, um die geänderten Zeilen zu ermitteln. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Der Spitzname der veröffentlichten Tabelle.|  
|**ROWGUID**|**uniqueidentifier**|Der Zeilenbezeichner für die angegebene Zeile.|  
|**Stro**|**bigint**|Die Generierung der Zeile, die durch **tablenick** und **ROWGUID** identifiziert wird.|  
|**partchangegen**|**bigint**|Die Generierung, die der letzten Datenänderung zugeordnet ist, bei der die Zugehörigkeit der Zeile zu einer gefilterten Veröffentlichung geändert worden sein könnte|  
|**Leitung**|**varbinary (501)**|Die Paare aus Spitzname des Abonnenten und Versionsnummer, die zur Verwaltung eines Verlaufs der Änderungen an dieser Zeile verwendet werden.|  
|**colvl**|**varbinary (7489)**|Die Versionsinformationen für die Spalte.|  
|**setzen**|**uniqueidentifier**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identifiziert die übergeordnete Zeile der obersten Ebene in **MSmerge_contents** (von **ROWGUID**) für jede zugehörige untergeordnete Zeile in einem logischen Datensatz.|  
|**logical_record_lineage**|**varbinary (501)**|Die Paare aus Spitzname des Abonnenten und Versionsnummer, die zur Verwaltung eines Verlaufs der Änderungen an der übergeordneten Zeile der obersten Ebene in einem logischen Datensatz verwendet werden. Für alle untergeordneten Zeilen in einem logischen Datensatz lautet dieser Wert NULL.|  
|**logical_relation_change_gen**|**bigint**|Der Generierungswert, der mit der letzten Änderung verbunden ist, die zur Neuausrichtung des logischen Datensatzes führte. Die Neuausrichtung wurde erforderlich, da eine vorhandene Zeile in den logischen Datensatz hinein oder aus diesem heraus verschoben wurde.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
