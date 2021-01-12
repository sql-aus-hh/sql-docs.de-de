---
description: DROP FULLTEXT INDEX (Transact-SQL)
title: DROP FULLTEXT INDEX (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_FULLTEXT_INDEX_TSQL
- DROP FULLTEXT INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- deleting full-text indexes
- removing full-text indexes
- full-text indexes [SQL Server], removing
- DROP FULLTEXT INDEX statement
- dropping full-text indexes
ms.assetid: 7443a4ab-1d43-4a22-bbba-a07f620892cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a5ed7a438741e9e69f5d5df98f9b5a3958977e26
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091946"
---
# <a name="drop-fulltext-index-transact-sql"></a>DROP FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt einen Volltextindex aus einer angegebenen Tabelle oder einer indizierten Sicht.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP FULLTEXT INDEX ON table_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *table_name*  
 Der Name der Tabelle oder indizierten Sicht, die den zu entfernenden Volltextindex enthält.  
  
## <a name="remarks"></a>Bemerkungen  
 Es ist nicht nötig, alle Spalten aus dem Volltextindex zu löschen, bevor Sie den DROP FULLTEXT INDEX-Befehl verwenden.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer muss über die ALTER-Berechtigung für die Tabelle oder die indizierte Sicht verfügen oder ein Mitglied der festen Serverrolle **sysadmin** oder der festen Datenbankrollen **db_owner** oder **db_ddladmin** sein.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der in der `JobCandidate`-Tabelle enthaltene Volltextindex gelöscht.  
  
```sql  
USE AdventureWorks2012;  
GO  
DROP FULLTEXT INDEX ON HumanResources.JobCandidate;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Volltextsuche](../../relational-databases/search/full-text-search.md)  
  
  
