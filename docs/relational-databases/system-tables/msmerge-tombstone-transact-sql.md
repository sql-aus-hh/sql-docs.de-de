---
description: MSmerge_tombstone (Transact-SQL)
title: MSmerge_tombstone (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_tombstone_TSQL
- MSmerge_tombstone
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_tombstone system table
ms.assetid: 8b3fc7bf-729b-40f2-8a26-e7dfbe8ddb38
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b8f54ce66145ddc298bb44594c48f74ddd62be0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098596"
---
# <a name="msmerge_tombstone-transact-sql"></a>MSmerge_tombstone (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_tombstone** Tabelle enthält Informationen zu gelöschten Zeilen und ermöglicht das Weitergeben von Lösch Vorgängen an andere Abonnenten. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**ROWGUID**|**uniqueidentifier**|Der Zeilen Bezeichner.|  
|**tablenick**|**int**|Spitzname der Tabelle|  
|**type**|**tinyint**|Der Typ des Löschvorgangs:<br /><br /> 1 = Löschvorgang durch Benutzer.<br /><br /> 5 = Zeile gehört nicht mehr zur gefilterten Partition.<br /><br /> 6 = Löschvorgang durch System.|  
|**Leitung**|**varbinary (249)**|Zeigt die Version des Datensatzes an, der gelöscht wurde, und die Updates, die bekannt waren, als der Datensatz gelöscht wurde. Ermöglicht Regeln für eine konsistente Auflösung eines Konflikts, wenn ein Abonnent eine Zeile aktualisiert, während diese auf einem anderen Abonnenten gelöscht wird.|  
|**Stro**|**int**|Wird zugewiesen, wenn eine Zeile gelöscht wird. Wenn ein Abonnent Generation N anfordert, werden nur Tombstones mit Generation >= n gesendet.|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identifiziert den logischen Datensatz, zu dem eine gelöschte Zeile gehört hat.|  
|**logical_record_lineage**|**Varbinary (501)**|Paare aus Spitzname des Abonnenten und Versionsnummer, die zur Verwaltung eines Verlaufs der Löschungen für den logischen Datensatz, zu dem diese Zeile gehört, verwendet werden.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
