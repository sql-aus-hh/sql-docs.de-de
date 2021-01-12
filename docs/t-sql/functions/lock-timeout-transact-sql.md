---
description: '&#x40;&#x40;LOCK_TIMEOUT (Transact-SQL)'
title: '@@LOCK_TIMEOUT (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 09/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@LOCK_TIMEOUT'
- '@@LOCK_TIMEOUT_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- '@@LOCK_TIMEOUT function'
- current lock time-out setting
- locking [SQL Server], time-outs
ms.assetid: 6bf8bf97-60b8-40c1-b89d-8f5a00bcae2e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3e1d70d12638f77437642a2b7738fea0d5f24129
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101042"
---
# <a name="x40x40lock_timeout-transact-sql"></a>&#x40;&#x40;LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt die aktuelle Einstellung für das Sperrtimeout für die aktuelle Sitzung in Millisekunden zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
@@LOCK_TIMEOUT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 **integer**  
  
## <a name="remarks"></a>Hinweise  
 SET LOCK_TIMEOUT ermöglicht einer Anwendung das Festlegen der maximalen Zeit, die eine Anweisung auf eine blockierte Ressource wartet. Wenn eine Anweisung länger als die Einstellung für LOCK_TIMEOUT gewartet hat, wird die blockierte Anweisung automatisch abgebrochen und eine Fehlermeldung an die Anwendung zurückgegeben.  
  
 @@LOCK_TIMEOUT gibt den Wert –1 zurück, wenn SET LOCK_TIMEOUT noch nicht in der aktuellen Sitzung ausgeführt wurde.  
  
## <a name="examples"></a>Beispiele  
 In diesem Beispiel wird das Resultset dargestellt, wenn kein Wert für LOCK_TIMEOUT festgelegt wurde.  
  
```sql  
SELECT @@LOCK_TIMEOUT AS [Lock Timeout];  
GO  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
Lock Timeout  
------------  
-1  
```  
  
 In diesem Beispiel wird LOCK_TIMEOUT auf 1800 Millisekunden festgelegt und dann @@LOCK_TIMEOUT aufgerufen.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
SELECT @@LOCK_TIMEOUT AS [Lock Timeout];  
GO  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
Lock Timeout  
------------  
1800          
```  
  
## <a name="see-also"></a>Siehe auch  
 [Konfigurationsfunktionen (Transact-SQL)](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SET LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/statements/set-lock-timeout-transact-sql.md)  
  
  
