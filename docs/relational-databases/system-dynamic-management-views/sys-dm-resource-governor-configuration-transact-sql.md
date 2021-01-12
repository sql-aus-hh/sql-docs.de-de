---
description: sys.dm_resource_governor_configuration (Transact-SQL)
title: sys.dm_resource_governor_configuration (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_resource_governor_configuration_TSQL
- dm_resource_governor_configuration
- sys.dm_resource_governor_configuration
- sys.dm_resource_governor_configuration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_configuration dynamic management view
ms.assetid: c89aab6a-0434-4ce6-af8c-f8a1a3284e38
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e49c5a5c5fde8bcbb75393146420323b6bf1c8d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098823"
---
# <a name="sysdm_resource_governor_configuration-transact-sql"></a>sys.dm_resource_governor_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile mit dem aktuellen Konfigurationsstatus der Ressourcenkontrolle im Arbeitsspeicher zurück.  
  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|classifier_function_id|**int**|Die ID der Klassifizierungsfunktion, die zurzeit von der Ressourcenkontrolle verwendet wird. Gibt den Wert 0 (null) zurück, wenn keine Funktion verwendet wird. Lässt keine NULL-Werte zu.<br /><br /> **Hinweis:** Diese Funktion wird zum Klassifizieren neuer Anforderungen verwendet und verwendet Regeln, um diese Anforderungen an die entsprechende Arbeits Auslastungs Gruppe weiterzuleiten. Weitere Informationen finden Sie unter [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).|  
|is_reconfiguration_pending|**bit**|Gibt an, ob mit der ALTER RESOURCE GOVERNOR RECONFIGURE-Anweisung Änderungen an einer Gruppe oder einem Pool vorgenommen wurden, die jedoch noch nicht auf die Konfiguration im Arbeitsspeicher angewendet wurden. Einer der folgenden Werte wird zurückgegeben:<br /><br /> 0 - Eine Anweisung zur Neukonfiguration ist nicht erforderlich.<br /><br /> 1 - Es ist eine Anweisung zur Neukonfiguration oder ein Serverneustart erforderlich, damit ausstehende Konfigurationsänderungen übernommen werden können.<br /><br /> **Hinweis:** Der zurückgegebene Wert ist immer 0, wenn Resource Governor deaktiviert ist.<br /><br /> Lässt keine NULL-Werte zu.|  
|max_outstanding_io_per_volume|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die maximale Anzahl der ausstehenden E/A-Vorgänge pro Volume.|  
  
## <a name="remarks"></a>Bemerkungen  
 Diese dynamische Verwaltungssicht zeigt die Konfiguration im Arbeitsspeicher an. Verwenden Sie die entsprechende Katalogsicht, um die gespeicherten Konfigurationsmetadaten anzuzeigen.  
  
 Das folgende Beispiel veranschaulicht, wie die gespeicherten Metadatenwerte und die Arbeitsspeicherwerte der Ressourcenkontrollenkonfiguration abgerufen und verglichen werden.  
  
```  
USE master;  
go  
-- Get the stored metadata.  
SELECT   
object_schema_name(classifier_function_id) AS 'Classifier UDF schema in metadata',   
object_name(classifier_function_id) AS 'Classifier UDF name in metadata'  
FROM   
sys.resource_governor_configuration;  
go  
-- Get the in-memory configuration.  
SELECT   
object_schema_name(classifier_function_id) AS 'Active classifier UDF schema',   
object_name(classifier_function_id) AS 'Active classifier UDF name'  
FROM   
sys.dm_resource_governor_configuration;  
go  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.resource_governor_configuration &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-configuration-transact-sql.md)   
 [Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor.md)  
  
  

