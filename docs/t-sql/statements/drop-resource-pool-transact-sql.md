---
description: DROP RESOURCE POOL (Transact-SQL)
title: DROP RESOURCE POOL (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP RESOURCE POOL
- DROP_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP RESOURCE POOL
ms.assetid: 18cd6dd9-7a6d-4a08-b9d5-649af23583d5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3de30be9f5a0fc2b7aa98f4201320f1811ae30f9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081278"
---
# <a name="drop-resource-pool-transact-sql"></a>DROP RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht einen benutzerdefinierten Ressourcenpool der Ressourcenkontrolle.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP RESOURCE POOL pool_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *pool_name*  
 Der Name eines vorhandenen benutzerdefinierten Ressourcenpools.  
  
## <a name="remarks"></a>Bemerkungen  
 Ressourcenpools mit Arbeitsauslastungsgruppen können nicht gelöscht werden.  
  
 Standardpools oder interne Pools der Ressourcenkontrolle können nicht gelöscht werden.  
  
 Sie sollten bei der Ausführung von DDL-Anweisungen mit dem Status der Ressourcenkontrolle vertraut sein. Weitere Informationen finden Sie unter [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL SERVER-Berechtigung.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Ressourcenpool mit dem Namen `big_pool` gelöscht.  
  
```sql  
DROP RESOURCE POOL big_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
