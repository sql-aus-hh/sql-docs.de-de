---
description: DROP PARTITION FUNCTION (Transact-SQL)
title: DROP PARTITION FUNCTION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP PARTITION FUNCTION
- DROP_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting partition functions
- DROP PARTITION FUNCTION statement
- partition functions [SQL Server], removing
- dropping partition functions
- removing partition functions
ms.assetid: a4bb055a-a538-4db9-a6fb-550d1eabfa18
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 748c1832acb1f6dee86086f15ea21b23603f38f8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096953"
---
# <a name="drop-partition-function-transact-sql"></a>DROP PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt eine Partitionsfunktion aus der aktuellen Datenbank. Partitionsfunktionen werden mithilfe von CREATE PARTITION FUNCTION erstellt und mithilfe von ALTER PARTITION FUNCTION geändert.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql 
DROP PARTITION FUNCTION partition_function_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *partition_function_name*  
 Der Name der Partitionsfunktion, die gelöscht werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Löschen einer Partitionsfunktion ist nur möglich, wenn sie zum Zeitpunkt des Löschens von keinem Partitionsschema verwendet wird. Wenn Partitionsschemas vorhanden sind, die die Partitionsfunktion verwenden, gibt DROP PARTITION FUNCTION einen Fehler zurück.  
  
## <a name="permissions"></a>Berechtigungen  
 Jede der folgenden Berechtigungen ermöglicht das Ausführen von DROP PARTITION FUNCTION.  
  
-   ALTER ANY DATASPACE-Berechtigung. Diese Berechtigung gilt standardmäßig für Mitglieder der festen Serverrolle **sysadmin** und für Mitglieder der festen Datenbankrollen **db_owner** und **db_ddladmin** .  
  
-   Die Berechtigung CONTROL oder ALTER für die Datenbank, in der die Partitionsfunktion erstellt wurde.  
  
-   Die Berechtigung CONTROL SERVER oder ALTER ANY DATABASE auf dem Server der Datenbank, in der die Partitionsfunktion erstellt wurde.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird davon ausgegangen, dass die Partitionsfunktion `myRangePF` in der aktuellen Datenbank erstellt wurde.  
  
```sql 
DROP PARTITION FUNCTION myRangePF;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
 [sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
