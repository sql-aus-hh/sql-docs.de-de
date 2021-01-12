---
description: MSmerge_partition_groups (Transact-SQL)
title: MSmerge_partition_groups (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_partition_groups_TSQL
- MSmerge_partition_groups
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_partition_groups system table
ms.assetid: 5d56d780-ee40-4afc-9c2a-d1723d86e430
author: cawrites
ms.author: chadam
ms.openlocfilehash: 823e2a57cd6292be59fe9dca7bb805b8eaa47d49
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098651"
---
# <a name="msmerge_partition_groups-transact-sql"></a>MSmerge_partition_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_partition_groups** Tabelle speichert eine Zeile für jede Voraus berechnete Partition in einer bestimmten Datenbank. Neben den aufgelisteten Spalten wird dieser Tabelle eine Spalte für jede Funktion hinzugefügt, die in einem parametrisierten Zeilenfilter verwendet wird. Beispielsweise wird der Tabelle eine Spalte mit dem Namen **HOST_NAME_FN** hinzugefügt, wenn ein Filter die [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) -Funktion verwendet. Eine Zeile wird für jeden eindeutigen Funktionswertesatz gespeichert, der mit diesem Verleger synchronisiert wurde. Wenn mindestens zwei Abonnenten mit genau dem gleichen Wert für alle diese Funktionen synchronisiert werden, nutzen sie gemeinsam dieselbe Zeile in dieser Tabelle und deshalb auch die gleiche Partitions-ID. Diese Tabelle wird in der Veröffentlichungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**partition_id**|**int**|Die Identitätsspalte, die eine eindeutige ID für die vorausberechnete Partition bereitstellt.|  
|**publication_number**|**smallint**|Die Veröffentlichungsnummer, die in **sysmergepublications** gespeichert wird.|  
|**maxgen_whenadded**|**bigint**|Die höchste bekannte Generierung auf dem Verleger zu dem Zeitpunkt, als die Zeile in dieser Tabelle eingefügt wurde.|  
|**using_partition_groups**|**bit**|Gibt an, ob die Partition zu einer Veröffentlichung gehört, die vorausberechnete Partitionen verwendet. Die folgenden Werte sind möglich:<br /><br /> **0** = die Veröffentlichung verwendet keine voraus berechneten Partitionen.<br /><br /> **1** = Veröffentlichung verwendet Voraus berechnete Partitionen<br /><br /> Weitere Informationen finden Sie unter [Optimieren Parametrisierter Filter-Leistung mit Vorausberechneten Partitionen ](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).|  
|**HOST_NAME_FN**|**nvarchar(128)**|Der bereitgestellte Wert, wenn parametrisierte Zeilenfilter zum Generieren von Partitionen verwendet werden. Weitere Informationen zu parametrisierten Zeilenfiltern finden Sie unter [Parametrisierte Zeilenfilter](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
