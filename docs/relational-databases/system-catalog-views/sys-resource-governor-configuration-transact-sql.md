---
description: sys.resource_governor_configuration (Transact-SQL)
title: sys.resource_governor_configuration (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_configuration_TSQL
- sys.resource_governor_configuration
- resource_governor_configuration_TSQL
- resource_governor_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_configuration catalog view
ms.assetid: 89099668-1dc6-4b07-9d8b-49bc95c7bfc0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34e8a269a2c8e7fafd0ba0e6645e2363eefa5d77
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097972"
---
# <a name="sysresource_governor_configuration-transact-sql"></a>sys.resource_governor_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den gespeicherten Status der Ressourcenkontrolle zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|classifier_function_id|**int**|Die ID der Klassifizierungsfunktion, wie sie in den Metadaten gespeichert ist. Lässt keine NULL-Werte zu.<br /><br /> **Hinweis** Diese Funktion wird zum Klassifizieren neuer Sitzungen verwendet und verwendet Regeln, um die Arbeitsauslastung an die entsprechende Arbeits Auslastungs Gruppe weiterzuleiten. Weitere Informationen finden Sie unter [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).|  
|is_enabled|**bit**|Gibt den aktuellen Status der Ressourcenkontrolle an:<br /><br /> 0 = Resource Governor ist nicht aktiviert.<br /><br /> 1 = Resource Governor aktiviert ist.<br /><br /> Lässt keine NULL-Werte zu.|  
|max_outstanding_io_per_volume|**int**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Die maximale Anzahl der ausstehenden E/A-Vorgänge pro Volume.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Katalogsicht zeigt die Konfiguration der Ressourcenkontrolle an, wie sie in den Metadaten gespeichert ist. Um die Konfiguration im Arbeitsspeicher anzuzeigen, verwenden Sie die entsprechende dynamische Verwaltungssicht.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW ANY DEFINITION-Berechtigung, um Inhalte anzuzeigen, und erfordert die CONTROL SERVER-Berechtigung, um Inhalte zu ändern.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel veranschaulicht, wie die gespeicherten Metadatenwerte und die Arbeitsspeicherwerte der Ressourcenkontrollenkonfiguration abgerufen und verglichen werden.  
  
```  
USE master;  
GO  
-- Get the stored metadata.  
SELECT   
object_schema_name(classifier_function_id) AS 'Classifier UDF schema in metadata',   
object_name(classifier_function_id) AS 'Classifier UDF name in metadata'  
FROM   
sys.resource_governor_configuration;  
GO  
-- Get the in-memory configuration.  
SELECT   
object_schema_name(classifier_function_id) AS 'Active classifier UDF schema',   
object_name(classifier_function_id) AS 'Active classifier UDF name'  
FROM   
sys.dm_resource_governor_configuration;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Resource Governor Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_configuration &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md)   
 [Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor.md)  
  
  
