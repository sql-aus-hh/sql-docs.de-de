---
description: sys.dm_clr_loaded_assemblies (Transact-SQL)
title: sys.dm_clr_loaded_assemblies (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_clr_loaded_assemblies
- sys.dm_clr_loaded_assemblies_TSQL
- dm_clr_loaded_assemblies_TSQL
- sys.dm_clr_loaded_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_loaded_assemblies dynamic management view
ms.assetid: 8523d8db-d8a0-4b1f-ae19-6705d633e0a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d132a0389ca636dbc24afe016b98389021d8d848
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099954"
---
# <a name="sysdm_clr_loaded_assemblies-transact-sql"></a>sys.dm_clr_loaded_assemblies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt für jede in den Serveradressraum geladene verwaltete Benutzerassembly eine Zeile zurück. Verwenden Sie diese Ansicht, um die CLR-Integration verwaltete Datenbankobjekte, die in ausgeführt werden, zu verstehen und zu beheben [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Assemblys stellen DLL-Dateien mit verwaltetem Code dar, die zum Definieren und Bereitstellen von verwalteten Datenbankobjekten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet werden. Wenn ein Benutzer eines dieser verwalteten Datenbankobjekte ausführt, wird die Assembly (und ihre Verweise), in der das verwaltete Datenbankobjekt definiert wird, von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und der CLR geladen. Die Assembly bleibt in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geladen, damit die Leistung optimiert wird, sodass die in der Assembly enthaltenen verwalteten Datenbankobjekte in Zukunft aufgerufen werden können, ohne dass die Assembly neu geladen werden muss. Die Assembly wird erst entladen, wenn für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr genügend Arbeitsspeicher vorhanden ist. Weitere Informationen zu Assemblys und zur CLR-Integration finden Sie unter [CLR Hosted Environment](../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md). Weitere Informationen zu verwalteten Datenbankobjekten finden Sie unter [Building Database Objects with Common Language Runtime &#40;CLR&#41; Integration](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID der geladenen Assembly. Der **assembly_id** kann verwendet werden, um weitere Informationen zur Assembly in der Katalog Sicht [sys. Assemblys &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) zu suchen. Beachten Sie, dass im [!INCLUDE[tsql](../../includes/tsql-md.md)] [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) -Katalog nur Assemblys in der aktuellen Datenbank angezeigt werden. Mit der **sqs.dm_clr_loaded_assemblies** -Sicht werden alle geladenen Assemblys auf dem Server angezeigt.|  
|**appdomain_address**|**int**|Adresse der Anwendungsdomäne (**AppDomain**), in der die Assembly geladen wird. Alle Assemblys, die sich im Besitz eines einzelnen Benutzers befinden, werden stets in derselben **AppDomain** geladen. **appdomain_address** kann für die Suche nach weiteren Informationen zur **AppDomain** in der [sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md) -Sicht verwendet werden.|  
|**load_time**|**datetime**|Zeit, zu der die Assembly geladen wurde. Beachten Sie, dass die Assembly geladen bleibt, bis nicht mehr genügend Arbeitsspeicher in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorhanden ist und die **AppDomain** entladen wird. Sie können **load_time** überwachen, um zu ermitteln, wie häufig in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht genügend Arbeitsspeicher vorhanden ist und die **AppDomain** entladen wird.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="remarks"></a>Bemerkungen  
 Die **dm_clr_loaded_assemblies.appdomain_address** -Sicht besitzt eine n:1-Beziehung mit  **dm_clr_appdomains.appdomain_address**. Die **dm_clr_loaded_assemblies.assembly_id** -Sicht besitzt eine 1:n-Beziehung mit **sys.assemblies.assembly_id**.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird veranschaulicht, wie Details zu allen Assemblys in der aktuellen Datenbank angezeigt werden können, die aktuell geladen sind.  
  
```  
 SELECT a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
FROM sys.dm_clr_loaded_assemblies AS l   
INNER JOIN sys.assemblies AS a  
ON l.assembly_id = a.assembly_id;  
```  
  
 Im folgenden Beispiel wird veranschaulicht, wie Details der **AppDomain** angezeigt werden können, in der eine bestimmte Assembly geladen ist.  
  
```  
SELECT appdomain_id, creation_time, db_id, user_id, state  
FROM sys.dm_clr_appdomains AS a  
WHERE appdomain_address =   
(SELECT appdomain_address   
 FROM sys.dm_clr_loaded_assemblies  
 WHERE assembly_id = 555);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Common Language Runtime &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
