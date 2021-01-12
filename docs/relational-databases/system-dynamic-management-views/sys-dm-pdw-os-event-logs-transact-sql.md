---
description: sys.dm_pdw_os_event_logs (Transact-SQL)
title: sys.dm_pdw_os_event_logs (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a0daa8cf-72e2-4349-8be1-d3cc0f9b1e02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: af5633fc5e1a169c71197ac77a715e4b28da2b57
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092696"
---
# <a name="sysdm_pdw_os_event_logs-transact-sql"></a>sys.dm_pdw_os_event_logs (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Enthält Informationen zu den verschiedenen Windows-Ereignisprotokollen auf den verschiedenen Knoten.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Der Geräteknoten, von dem dieses Protokoll abgeleitet ist.<br /><br /> pdw_node_id und log_name den Schlüssel für diese Ansicht bilden.||  
|log_name|**nvarchar(255)**|Der Name des Windows-Ereignis Protokolls.<br /><br /> pdw_node_id und log_name den Schlüssel für diese Ansicht bilden.||  
|log_source|**nvarchar(255)**|Quellen Name des Windows-Ereignis Protokolls.||  
|event_id|**int**|ID des Ereignisses. Nicht eindeutig.||  
|event_type|**nvarchar(255)**|Der Typ des Ereignisses, der den Schweregrad identifiziert.|"Information", "Warning", "Error"|  
|event_message|**nvarchar(4000)**|Details zum Ereignis.||  
|generate_time|**datetime**|Zeitpunkt, zu dem das Ereignis erstellt wurde.||  
|write_time|**datetime**|Uhrzeit, zu der das Ereignis tatsächlich in das Protokoll geschrieben wurde.||  
  
 Informationen über die maximale Anzahl von Zeilen, die in dieser Sicht beibehalten werden, finden Sie im Abschnitt "Metadaten" im Thema [Kapazitäts Limits](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) . 
  
## <a name="see-also"></a>Weitere Informationen  
 [Azure Synapse Analytics und parallele Data Warehouse dynamische Verwaltungs Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
