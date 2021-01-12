---
description: MSdynamicsnapshotjobs (Transact-SQL)
title: MSdynamicsnapshotjobs (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdynamicsnapshotjobs_TSQL
- MSdynamicsnapshotjobs
dev_langs:
- TSQL
helpviewer_keywords:
- MSdynamicsnapshotjobs system table
ms.assetid: 4f36a325-0e3c-46c4-aeeb-416346cce0bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: aadba278ff96ed4b9102ad748f86865e6072c2f0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094780"
---
# <a name="msdynamicsnapshotjobs-transact-sql"></a>MSdynamicsnapshotjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In der **MSdynamicsnapshotjobs** -Tabelle werden die parametrisierten Zeilen Filter Informationen nachverfolgt, die zum Generieren einer gefilterten Daten Momentaufnahme angewendet wurden. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Die ID des gefilterten Datenmomentaufnahmeauftrags.|  
|**name**|**sysname**|Der Name des gefilterten Datenmomentaufnahmeauftrags.|  
|**pubid**|**uniqueidentifier**|Die eindeutige ID für diese Veröffentlichung.|  
|**job_id**|**uniqueidentifier**|Die ID des Auftrags des SQL Server-Agents auf dem Verteiler.|  
|**agent_id**|**int**|Die ID des SQL Server-Agents.|  
|**dynamic_filter_login**|**sysname**|Der Wert, der zum Auswerten der [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) Funktion in parametrisierten Zeilen filtern verwendet wird, die für die Veröffentlichung definiert sind.|  
|**dynamic_filter_hostname**|**sysname**|Der Wert, der zum Auswerten der [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) Funktion in parametrisierten Zeilen filtern verwendet wird, die für die Veröffentlichung definiert sind.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Der Pfad zu dem Ordner, aus dem die Momentaufnahmedateien gelesen werden, wenn eine gefilterte Datenmomentaufnahme verwendet wird.|  
|**partition_id**|**int**|Die ID der Datenpartition, der der Auftrag angehört.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikationstabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
