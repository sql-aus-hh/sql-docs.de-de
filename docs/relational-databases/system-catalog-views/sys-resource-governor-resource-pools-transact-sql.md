---
description: sys.resource_governor_resource_pools (Transact-SQL)
title: sys.resource_governor_resource_pools (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_resource_pools
- resource_governor_resource_pools
- sys.resource_governor_resource_pools_TSQL
- resource_governor_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_resource_pools catalog view
ms.assetid: 56793e9c-aa90-452e-88c6-d9b799239888
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ffab459e53f26d040c301adf818618b28fa05f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101779"
---
# <a name="sysresource_governor_resource_pools-transact-sql"></a>sys.resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt die gespeicherte Konfiguration der Ressourcenpools in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zurück. Jede Zeile der Sicht bestimmt die Konfiguration eines Pools.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|Eindeutige ID des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|name|**sysname**|Name des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|min_cpu_percent|**int**|Garantierte durchschnittliche CPU-Bandbreite für alle Anforderungen im Ressourcenpool, wenn CPU-Konflikte bestehen. Lässt keine NULL-Werte zu.|  
|max_cpu_percent|**int**|Maximal zulässige durchschnittliche CPU-Bandbreite für alle Anforderungen im Ressourcenpool an, wenn CPU-Konflikte bestehen. Lässt keine NULL-Werte zu.|  
|min_memory_percent|**int**|Garantierte Arbeitsspeichermenge für alle Anforderungen im Ressourcenpool. Dieser Arbeitsspeicher wird nicht mit anderen Ressourcenpools gemeinsam genutzt. Lässt keine NULL-Werte zu.|  
|max_memory_percent|**int**|Prozentsatz des gesamten Serverspeichers, der für Anforderungen in diesem Ressourcenpool verwendet werden kann. Lässt keine NULL-Werte zu. Der geltende Höchstwert hängt von den Pool-Mindestwerten ab. So kann für max_memory_percent beispielsweise 100 festgelegt sein, aber der geltende Höchstwert ist niedriger.|  
|cap_cpu_percent|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Feste Obergrenze der CPU-Bandbreite, die allen Anforderungen im Ressourcenpool zugewiesen wird. Beschränkt die maximale CPU-Bandbreite auf die angegebene Stufe. Der zulässige Bereich für den Wert ist 1 bis 100.|  
|min_iops_per_volume|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die minimalen E/A-Vorgänge pro Sekunde (IOPS) pro Volumeeinstellung für diesen Pool. 0 = keine Reservierung. Darf nicht NULL sein.|  
|max_iops_per_volume|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die maximalen E/A-Vorgänge pro Sekunde (IOPS) pro Volumeeinstellung für diesen Pool. 0 = unbegrenzt. Darf nicht NULL sein.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Katalogsicht zeigt die gespeicherten Metadaten an. Um die Konfiguration im Arbeitsspeicher anzuzeigen, verwenden Sie die entsprechende dynamische Verwaltungs Sicht, [sys.dm_resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW ANY DEFINITION-Berechtigung, um Inhalte anzuzeigen, und erfordert die CONTROL SERVER-Berechtigung, um Inhalte zu ändern.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Resource Governor Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)  
  
  
