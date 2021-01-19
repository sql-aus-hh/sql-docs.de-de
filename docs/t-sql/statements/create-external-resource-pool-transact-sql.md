---
description: CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
title: CREATE EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL RESOURCE POOL
- CREATE EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE POOL
- EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE
- EXTERNAL_RESOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL RESOURCE POOL statement
ms.assetid: 8cc798ad-c395-461c-b7ff-8c561c098808
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 7f57b0e829c240813d6f1a453d764e64bce6b279
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171602"
---
# <a name="create-external-resource-pool-transact-sql"></a>CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Erstellt einen externen Pool, mit dem Ressourcen für externe Prozesse definiert werden. Ein Ressourcenpool stellt eine Teilmenge der physischen Ressourcen (Arbeitsspeicher und CPUs) einer Datenbank-Engine-Instanz dar. Ein Resource Governor kann Serverressourcen auf Ressourcenpools verteilen. Bis zu 64 Pools sind möglich.

::: moniker range="=sql-server-2016"
Für [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] kontrolliert der externe Pool `rterm.exe`, `BxlServer.exe` und andere Prozesse, die von diesen Anwendungen erzeugt wurden.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Für [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] kontrolliert der externe Pool `rterm.exe`, `python.exe`, `BxlServer.exe` und andere Prozesse, die von diesen Anwendungen erzeugt wurden.
::: moniker-end
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
 

## <a name="syntax"></a>Syntax  
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"

```syntaxsql
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```syntaxsql
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] AFFINITY CPU =    
            {  
                AUTO   
              | ( <cpu_range_spec> )   
              | NUMANODE = ( <NUMA_node_id> )   
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*pool_name*  
Der benutzerdefinierte Name für den externen Ressourcenpool. *pool_name* ist alphanumerisch und kann bis zu 128 Zeichen lang sein. Dieses Argument muss innerhalb einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eindeutig sein und die Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) erfüllen.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT = *value*  
Die maximale durchschnittliche CPU-Bandbreite für alle Anforderungen im externen Ressourcenpool, wenn CPU-Konflikte bestehen. *value* ist ein Integer. Der zulässige Bereich für *value* liegt zwischen 1 und 100.


MAX_MEMORY_PERCENT =*value*  
Gibt den gesamten Serverspeicher an, der für Anforderungen in diesem externen Ressourcenpool verwendet werden kann. *value* ist ein Integer. Der zulässige Bereich für *value* liegt zwischen 1 und 100.

MAX_PROCESSES = *value*  
Die maximale Anzahl von Prozessen, die für den externen Ressourcenpool zulässig sind. 0 entspricht einem unbegrenzten Schwellenwert für den Pool, der anschließend nur durch Computerressourcen gebunden ist.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT = *value*  
Die maximale durchschnittliche CPU-Bandbreite für alle Anforderungen im externen Ressourcenpool, wenn CPU-Konflikte bestehen. *value* ist ein Integer. Der zulässige Bereich für *value* liegt zwischen 1 und 100.

AFFINITY {CPU = AUTO | ( <angegebener_CPU-Bereich>) | NUMANODE = (\<NUMA_node_range_spec>)} fügt den externen Ressourcenpool an bestimmte CPUs an.

AFFINITY CPU = **(** <angegebener_CPU-Bereich> **)** ordnet den externen Ressourcenpool den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-CPUs zu, die durch die angegebenen CPU_IDs identifiziert werden.

Wenn Sie AFFINITY NUMANODE = **(\<NUMA_node_range_spec> **)** verwenden, wird für den externen Ressourcenpool die Affinität mit den physischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-CPUs festgelegt, die dem angegebenen NUMA-Knoten oder dem Knotenbereich entsprechen. 

MAX_MEMORY_PERCENT =*value*  
Gibt den gesamten Serverspeicher an, der für Anforderungen in diesem externen Ressourcenpool verwendet werden kann. *value* ist ein Integer. Der zulässige Bereich für *value* liegt zwischen 1 und 100.

MAX_PROCESSES = *value*  
Die maximale Anzahl von Prozessen, die für den externen Ressourcenpool zulässig sind. 0 entspricht einem unbegrenzten Schwellenwert für den Pool, der anschließend nur durch Computerressourcen gebunden ist.
::: moniker-end

## <a name="remarks"></a>Bemerkungen

Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] implementiert den Ressourcenpool, wenn Sie die [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md)-Anweisung ausführen.

Allgemeine Informationen zu Ressourcenpools finden Sie unter [Resource Governor-Ressourcenpool](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) und [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).

Informationen zur Verwaltung externer Ressourcenpools für Machine Learning finden Sie unter [Resource governance for machine learning in SQL Server (Ressourcenkontrolle für Machine Learning in SQL Server)](../../machine-learning/administration/resource-governor.md). 

## <a name="permissions"></a>Berechtigungen

Erfordert die `CONTROL SERVER`-Berechtigung.

## <a name="examples"></a>Beispiele

Für den externen Pool wurde die CPU-Auslastung auf 75 Prozent beschränkt. Der maximale Arbeitsspeicher beträgt 30 Prozent des verfügbaren Arbeitsspeichers auf dem Computer.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>Weitere Informationen

+ [Externe Skripts aktiviert (Serverkonfigurationsoption)](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Ressourcenpool für die Ressourcenkontrolle](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)
+ [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)
+ [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)
