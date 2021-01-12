---
description: DROP TYPE (Transact-SQL)
title: DROP TYPE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP TYPE
- DROP_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined types [SQL Server], deleting
- UDTs [SQL Server], deleting
- alias data types [SQL Server], removing
- DROP TYPE statement
ms.assetid: 11bf83f9-0718-4238-a835-83d2eb14ae7b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f7dc3dcd3f1fa87330bda0865160341eec462a86
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093434"
---
# <a name="drop-type-transact-sql"></a>DROP TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt einen Aliasdatentyp oder einen benutzerdefinierten CLR-Typ (Common Language Runtime) aus der aktuellen Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP TYPE [ IF EXISTS ] [ schema_name. ] type_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Löscht den Typ nur, wenn dieser bereits vorhanden ist.  
  
 *schema_name*  
 Der Name des Schemas, zu dem der Aliastyp oder benutzerdefinierte Typ gehört.  
  
 *type_name*  
 Der Name des Aliasdatentyps oder benutzerdefinierten Typs, den Sie löschen möchten.  
  
## <a name="remarks"></a>Hinweise  
 Die DROP TYPE-Anweisung wird nicht ausgeführt, wenn eine der folgenden Bedingungen zutrifft:  
  
-   In der Datenbank sind Tabellen vorhanden, die Spalten des Aliasdatentyps oder benutzerdefinierten Typs enthalten. Informationen zu Spalten mit Aliastypen oder benutzerdefinierten Typen erhalten Sie, indem Sie die Katalogsichten [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) oder [sys.column_type_usages](../../relational-databases/system-catalog-views/sys-column-type-usages-transact-sql.md) abfragen.  
  
-   Es sind berechnete Spalten, CHECK-Einschränkungen, schemagebundene Sichten und schemagebundene Funktionen enthalten, deren Definitionen auf den Aliastyp oder benutzerdefinierten Typ verweisen. Informationen zu diesen Verweisen erhalten Sie, indem Sie die [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)-Katalogsicht abfragen.  
  
-   Es sind in der Datenbank erstellte Funktionen, gespeicherte Prozeduren oder Trigger vorhanden, die Variablen und Parameter des Aliastyps oder benutzerdefinierten Typs verwenden. Informationen zu Parametern mit Aliastypen oder benutzerdefinierten Typen erhalten Sie, indem Sie die Katalogsichten [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md) oder [sys.parameter_type_usages](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md) abfragen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert entweder die CONTROL-Berechtigung für *type_name* oder die ALTER-Berechtigung für *schema_name*.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird davon ausgegangen, dass ein Typ namens `ssn` bereits in der aktuellen Datenbank erstellt wurde.  
  
```sql  
DROP TYPE ssn ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
