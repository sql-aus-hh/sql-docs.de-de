---
description: '&gt;= (größer oder gleich) (Transact-SQL)'
title: '&gt;= (größer oder gleich) (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Greater
- '>='
- '>= (Greater Than or Equal To)'
- Equal To
- '>=_TSQL'
- Greater Than
- Equal
dev_langs:
- TSQL
helpviewer_keywords:
- greater than or equal to operator (>=)
- '>= (greater than or equal to operator)'
ms.assetid: 641ee28d-7536-46dd-a48a-6c63c2d59278
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a028f0060dae9e3a62fea0b06b7843a1a72c363
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100362"
---
# <a name="gt-greater-than-or-equal-to-transact-sql"></a>&gt;= (größer oder gleich) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Vergleicht zwei Ausdrücke nach dem Kriterium größer als oder gleich (ein Vergleichsoperator).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
expression >= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *expression*  
 Ein beliebiger gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md). Beide Ausdrücke müssen implizit konvertierbare Datentypen besitzen. Die Konvertierung hängt von den [Rangfolgeregeln für Datentypen](../../t-sql/data-types/data-type-precedence-transact-sql.md) ab.  
  
## <a name="result-types"></a>Ergebnistypen  
 Boolean  
  
## <a name="remarks"></a>Hinweise  
 Beim Vergleich von Ausdrücken, die ungleich NULL sind, ist das Ergebnis TRUE, wenn der linke Operand einen größeren Wert besitzt als der rechte Operand oder einen Wert, der gleich groß ist; andernfalls ist das Ergebnis FALSE.  
  
 Im Gegensatz zum Vergleichsoperator = (gleich) ist das Ergebnis des Vergleichs zweier NULL-Werte mit dem Operator >= nicht von der ANSI_NULLS-Einstellung abhängig.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using--in-a-simple-query"></a>A. Verwenden von >= in einer einfachen Abfrage  
 Im folgenden Beispiel werden alle Zeilen in der `HumanResources.Department` -Tabelle zurückgegeben, die in `DepartmentID` über einen Wert größer oder gleich 13 verfügen.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name  
FROM HumanResources.Department  
WHERE DepartmentID >= 13  
ORDER BY DepartmentID;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
DepartmentID Name  
------------ --------------------------------------------------  
13           Quality Assurance  
14           Facilities and Maintenance  
15           Shipping and Receiving  
16           Executive  
  
(4 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [= &#40;Equals&#41; &#40;Transact-SQL&#41; (= &#40;ist gleich&#41; &#40;Transact-SQL&#41;)](../../t-sql/language-elements/equals-transact-sql.md)   
 [&#62; &#40;Greater Than&#41; &#40;Transact-SQL&#41; (&#62; &#40;Größer als&#41; &#40;Transact-SQL&#41;)](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
