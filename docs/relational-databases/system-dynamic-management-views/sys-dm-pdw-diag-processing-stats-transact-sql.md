---
description: sys.dm_pdw_diag_processing_stats (Transact-SQL)
title: sys.dm_pdw_diag_processing_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: df659c55-4f63-45f8-8afe-ce300031bc5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: a50a56e230d4074e567d63bea14744aa4dd18fd6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097523"
---
# <a name="sysdm_pdw_diag_processing_stats-transact-sql"></a>sys.dm_pdw_diag_processing_stats (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Zeigt Informationen zu allen internen Diagnose Ereignissen an, die in vom Administrator definierte Diagnose Sitzungen integriert werden könnten. Fragen Sie diese Ansicht ab, um die Statistiken hinter den Diagnose-und Ereignis Subsystemen zu verstehen, die die Auffüllung aller anderen DMVs steuern. Es gibt eine Gruppe von Warteschlangen für jeden Prozess auf jedem Knoten.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|Der Geräteknoten, von dem dieses Protokoll abgeleitet ist.|  
|**process_id**|**int**|Der Bezeichner des Prozesses, der die Übermittlung dieser Statistik durchführt.|  
|**target_name**|**nvarchar(255)**|Der Name der Warteschlange.|  
|**queue_size**|**int**|Die Anzahl der Elemente in der Verarbeitungs Warteschlange. Die Warteschlangen Größe beträgt normalerweise 0. Eine positive Zahl gibt an, dass das System ausgelastet ist und einen Rückstand an Ereignissen aufbaut. Eine positive Anzahl in den anderen Spalten bedeutet, dass das System für diese bestimmte Warteschlange und alle zugehörigen DMVs beschädigt wurde.|  
|**lost_events_count**|**bigint**|Die Anzahl der verlorenen Ereignisse.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Azure Synapse Analytics und parallele Data Warehouse dynamische Verwaltungs Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
