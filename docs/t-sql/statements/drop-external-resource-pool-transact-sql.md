---
description: DROP EXTERNAL RESOURCE POOL (Transact-SQL)
title: DROP EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- DROP EXTERNAL RESOURCE POOL
- DROP_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL RESOURCE POOL statement
ms.assetid: e2fa01bd-96ff-4ea9-bb08-6cb6b6adf68c
author: dphansen
ms.author: davidph
manager: cgronlund
ms.openlocfilehash: 08f25755ca4742b0e14379c40e22e99741eedbda
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172392"
---
# <a name="drop-external-resource-pool-transact-sql"></a>DROP EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Löscht einen externen Resource Governor-Ressourcenpool, der zum Definieren von Ressourcen für externe Prozesse verwendet wird. 

::: moniker range="=sql-server-2016||>=sql-server-linux-ver15"
Für [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] in [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] kontrolliert der externe Pool `rterm.exe`, `BxlServer.exe` und andere Prozesse, die von diesen Anwendungen erzeugt wurden.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Für [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] kontrolliert der externe Pool `rterm.exe`, `python.exe`, `BxlServer.exe` und andere Prozesse, die von diesen Anwendungen erzeugt wurden.
::: moniker-end

Externe Ressourcenpools werden mithilfe von [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md) erstellt und mithilfe von [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md) geändert.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP EXTERNAL RESOURCE POOL pool_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*pool_name*  
Der Name des externen Ressourcenpools, der gelöscht werden soll.  
  
## <a name="remarks"></a>Bemerkungen

Externe Ressourcenpools mit Arbeitsauslastungsgruppen können nicht gelöscht werden.  

Standardpools oder interne Pools der Ressourcenkontrolle können nicht gelöscht werden.  

Sie sollten bei der Ausführung von DDL-Anweisungen mit dem Status der Ressourcenkontrolle vertraut sein. Weitere Informationen finden Sie unter [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  

## <a name="permissions"></a>Berechtigungen

Erfordert die `CONTROL SERVER`-Berechtigung.  

## <a name="examples"></a>Beispiele

Im folgenden Beispiel wird der externe Ressourcenpool mit dem Namen `ex_pool` gelöscht.  

```sql
DROP EXTERNAL RESOURCE POOL ex_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Weitere Informationen

+ [Externe Skripts aktiviert (Serverkonfigurationsoption)](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)
+ [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)
