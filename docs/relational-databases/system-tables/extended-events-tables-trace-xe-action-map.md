---
description: 'Erweiterte Ereignistabelle: trace_xe_action_map'
title: trace_xe_action_map (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- trace_xe_action_map_TSQL
- trace_xe_action_map
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], tables
- trace_xe_action_map
ms.assetid: 208a1413-ce7f-4521-b765-d74723627302
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5eeb22a3617ffb2510c4d9d768cf2cde5e281ee0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100600"
---
# <a name="extended-events-tables---trace_xe_action_map"></a>Erweiterte Ereignistabelle: trace_xe_action_map
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Aktion für erweiterte Ereignisse, die der Spalten-ID für eine SQL-Ablaufverfolgung zugeordnet ist. Diese Tabelle wird in der Master-Datenbank im sys-Schema gespeichert.  
  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|trace_event_id|**smallint**|Die ID der Spalte für die SQL-Ablaufverfolgung, die zugeordnet wird.|  
|package_name|**nvarchar(60)**|Der Name des Pakets für erweiterte Ereignisse, in dem sich die zugeordnete Aktion befindet.|  
|xe_action_name|**nvarchar(60)**|Der Name der Aktion für erweiterte Ereignisse, die der Spalte für die SQL-Ablaufverfolgung zugeordnet ist.|  
  
## <a name="remarks"></a>Bemerkungen  
 Mit der folgenden Abfrage können Sie Aktionen für erweiterte Ereignisse identifizieren, die Spalten für die SQL-Ablaufverfolgung entsprechen:  
  
```  
SELECT tc.name AS trace_column, am.package_name, am.xe_action_name  
FROM sys.trace_columns AS tc  
INNER JOIN sys.trace_xe_action_map AS am  
   ON tc.trace_column_id = am.trace_column_id  
```  
  
 Spalten für die SQL-Ablaufverfolgung, die keinen Aktionen zugeordnet sind, sind nicht in der Tabelle enthalten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [trace_xe_event_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-event-map.md)  
  
  
