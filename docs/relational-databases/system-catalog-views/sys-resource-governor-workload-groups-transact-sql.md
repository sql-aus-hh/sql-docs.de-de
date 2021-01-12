---
description: sys.resource_governor_workload_groups (Transact-SQL)
title: sys.resource_governor_workload_groups (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_workload_groups
- resource_governor_workload_groups_TSQL
- sys.resource_governor_workload_groups_TSQL
- resource_governor_workload_groups
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_workload_groups catalog view
ms.assetid: 619ba4b7-868f-4784-b527-ec1dfd703c4f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 83d699e2da5e1de1e25a2618171924843c569188
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101756"
---
# <a name="sysresource_governor_workload_groups-transact-sql"></a>sys.resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt die gespeicherte Konfiguration der Arbeitsauslastungsgruppen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück. Jede Arbeitsauslastungsgruppe kann jeweils nur einen Ressourcenpool abonnieren.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|group_id|**int**|Eindeutige ID der Arbeitsauslastungsgruppe Lässt keine NULL-Werte zu.|  
|name|**sysname**|Name der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|  
|importance|**sysname**|**Hinweis:** Die Wichtigkeit gilt nur für Arbeits Auslastungs Gruppen im selben Ressourcenpool.<br /><br /> Die relative Wichtigkeit einer Anforderung in dieser Arbeitsauslastungsgruppe. Die Wichtigkeit ist eine der folgenden, wobei Medium die Standardeinstellung ist: niedrig, Mittel, hoch.<br /><br /> Lässt keine NULL-Werte zu.|  
|request_max_memory_grant_percent|**int**|Angabe der maximalen Arbeitsspeicherzuweisung in Prozent für eine einzelne Anforderung. Der Standardwert ist 25. Lässt keine NULL-Werte zu.<br /><br /> **Hinweis:** Wenn diese Einstellung höher als 50 Prozent ist, werden große Abfragen nacheinander ausgeführt. Daher besteht das Risiko, dass beim Ausführen der Abfrage nicht genügend Arbeitsspeicher verfügbar ist.|  
|request_max_cpu_time_sec|**int**|Maximaler CPU-Nutzungsgrenzwert für eine einzelne Anforderung in Sekunden. Der Standardwert 0 bedeutet, dass kein Grenzwert festgelegt ist. Lässt keine NULL-Werte zu.<br /><br /> **Hinweis:** Weitere Informationen finden Sie unter [CPU-Schwellenwert überschrittene Ereignisklasse](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|request_memory_grant_timeout_sec|**int**|Timeout für die Arbeitsspeicherzuweisung für eine einzelne Anforderung in Sekunden. Der Standardwert 0 verwendet eine interne Berechnung auf Basis der Abfragekosten. Lässt keine NULL-Werte zu.|  
|max_dop|**int**|Maximaler Grad der Parallelität für die Arbeitsauslastungsgruppe. Der Standardwert 0 verwendet globale Einstellungen. Lässt keine NULL-Werte zu.<br /><br /> **Knoten:** Mit dieser Einstellung wird die Abfrage Option **MAXDOP** überschrieben.|  
|group_max_requests|**int**|Maximale Anzahl gleichzeitiger Anforderungen. Der Standardwert 0 bedeutet, dass kein Grenzwert festgelegt ist. Lässt keine NULL-Werte zu.|  
|pool_id|**int**|ID des Ressourcenpools, den diese Arbeitsauslastungsgruppe verwendet.|  
|external_pool_id|**int**|**Gilt für**:  [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] und höher.<br /><br /> ID des externen Ressourcenpools, der von dieser Arbeits Auslastungs Gruppe verwendet wird.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Katalogsicht zeigt die gespeicherten Metadaten an. Um die Konfiguration im Arbeitsspeicher anzuzeigen, verwenden Sie die entsprechende dynamische Verwaltungs Sicht, [sys.dm_resource_governor_workload_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md).  
  
 Die gespeicherte Konfiguration und die Konfiguration im Arbeitsspeicher können sich unterscheiden, wenn die Konfiguration der Ressourcenkontrolle geändert wurde, die ALTER RESOURCE GOVERNOR RECONFIGURE-Anweisung jedoch nicht angewendet wurde.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW ANY DEFINITION-Berechtigung, um Inhalte anzuzeigen, und erfordert die CONTROL SERVER-Berechtigung, um Inhalte zu ändern.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Resource Governor Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)  
  
  
