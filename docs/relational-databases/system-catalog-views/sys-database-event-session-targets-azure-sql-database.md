---
description: sys.database_event_session_targets (Azure SQL-Datenbank)
title: sys.database_event_session_targets (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
ms.assetid: 38d775ee-1fe1-4820-88c6-02b2f875a66b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4f1c04a6556407b7aa8cb0ec046b2079e2a4b166
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102769"
---
# <a name="sysdatabase_event_session_targets-azure-sql-database"></a>sys.database_event_session_targets (Azure SQL-Datenbank)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Gibt für eine Ereignissitzung eine Zeile für jedes Ereignisziel zurück.  
  
||  
|-|  
|**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 und spätere Versionen.|  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Die ID der Ereignissitzung. Lässt keine NULL-Werte zu.|  
|target_id|**int**|Die ID des Ziels. Die ID ist innerhalb des Ereignissitzungsobjekts eindeutig. Lässt keine NULL-Werte zu.|  
|name|**sysname**|Der Name des Ereignisziels. Lässt keine NULL-Werte zu.|  
|Paket|**sysname**|Der Name des Ereignispakets, das das Ereignisziel enthält. Lässt keine NULL-Werte zu.|  
|module|**sysname**|Der Name des Moduls, das das Ereignisziel enthält. Lässt keine NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung auf dem Server.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Sicht hat die folgende Kardinalität der Beziehungen.  
  
|Von|Beschreibung|Relationship|  
|-|-|-|  
|sys.database_event_session_targets sys.database_event_session_targets.event_session_id|sys.database_event_sessions sys.database_event_sessions.event_session_id|n:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)  
  
  
