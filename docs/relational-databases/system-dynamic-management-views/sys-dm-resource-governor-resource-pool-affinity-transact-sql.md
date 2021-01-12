---
description: sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)
title: sys.dm_resource_governor_resource_pool_affinity (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pool_affinity_TSQL
- sys.dm_resource_governor_resource_pool_affinity
- dm_resource_governor_resource_pool_affinity
- dm_resource_governor_resource_pool_affinity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_resource_governor_resource_pool_affinity
- sys.dm_resource_governor_resource_pool_affinity
ms.assetid: a197ec19-a2ba-44f5-a4f2-3eee33ebd77d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d5fa2993e4ae716f72ad93bc388c466e69f00da3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097542"
---
# <a name="sysdm_resource_governor_resource_pool_affinity-transact-sql"></a>sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verfolgt die Ressourcenpoolaffinität.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
|Name des colmn|Datentyp|BESCHREIBUNG|  
|----------------|---------------|-----------------|  
|Pool_id|**int**|Die ID des Ressourcenpools. Lässt keine NULL-Werte zu.|  
|Processor_group|**smallint**|Die ID der logischen Windows-Prozessorgruppe. Lässt keine NULL-Werte zu.|  
|Scheduler_mask|**bigint**|Die binäre Maske, die für das diesem Pool zugeordnete Zeitplanungsmodul steht. Lässt keine NULL-Werte zu.|  
  
## <a name="remarks"></a>Bemerkungen  
 Pools, die mit der Affinität AUTO erstellt werden, werden in dieser Sicht nicht angezeigt, da sie keine Affinität besitzen. Weitere Informationen finden Sie in den Anweisungen zum [Erstellen eines Ressourcenpools &#40;Transact-SQL-&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md) und zum [Ändern von Ressourcenpools &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md) .  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)  
  
  
