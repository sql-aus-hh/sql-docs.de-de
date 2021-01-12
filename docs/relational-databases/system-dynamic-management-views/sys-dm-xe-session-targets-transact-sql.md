---
description: sys.dm_xe_session_targets (Transact-SQL)
title: sys.dm_xe_session_targets (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_session_targets
- dm_xe_session_targets_TSQL
- dm_xe_session_targets
- sys.dm_xe_session_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_session_targets dynamic management view
- extended events [SQL Server], views
ms.assetid: 76fbc3e1-ad88-4a47-8bf1-471c3bee5ad8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 041cf14bcefff0d294cd7c4f4f037870d1513552
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098870"
---
# <a name="sysdm_xe_session_targets-transact-sql"></a>sys.dm_xe_session_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu Sitzungszielen zurück.  
  
  |Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|Die Speicheradresse der Ereignissitzung. Hat eine n:1-Beziehung mit sys.dm_xe_sessions.address. Lässt keine NULL-Werte zu.|  
|target_name|**nvarchar(60)**|Der Name des Ziels innerhalb einer Sitzung. Lässt keine NULL-Werte zu.|  
|target_package_guid|**uniqueidentifier**|Die GUID des Pakets, das das Ziel enthält Lässt keine NULL-Werte zu.|  
|execution_count|**bigint**|Die Häufigkeit, mit der das Ziel für die Sitzung ausgeführt wurde. Lässt keine NULL-Werte zu.|  
|execution_duration_ms|**bigint**|Die gesamte Zeit in Millisekunden, für die das Ziel ausgeführt wurde. Lässt keine NULL-Werte zu.|  
|target_data|**nvarchar(max)**|Die Daten, die das Ziel beibehält, z. B. Ereignisaggregationsinformationen. Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
### <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_session_targets.event_session_address|sys.dm_xe_sessions.address|n:1|  
  
## <a name="change-history"></a>Änderungsverlauf  
  
|Aktualisierter Inhalt|  
|---------------------|  
|Der Datentyp für die target_data Spalte wurde korrigiert.|  
|Die Beschreibung für die target_data Spalte wurde korrigiert, um anzugeben, dass der Wert NULL-Werte zulässt.|  
|Die Tabelle "Kardinalität der Beziehungen" wurde korrigiert.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

