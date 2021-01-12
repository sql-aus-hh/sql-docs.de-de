---
description: NOT (Transact-SQL)
title: NOT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NOT_TSQL
- NOT
dev_langs:
- TSQL
helpviewer_keywords:
- negating Boolean input
- NOT operator [Transact-SQL]
- expressions [SQL Server], negating
- reversing Boolean expression values
ms.assetid: dc07cc35-20f1-46e6-9995-2938390dc19a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16f9160621608ac55cad00bec92391a47c457eba
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097116"
---
# <a name="not-transact-sql"></a>NOT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Negiert eine boolesche Eingabe.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
[ NOT ] boolean_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *boolean_expression*  
 Ein gültiger boolescher [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="result-types"></a>Ergebnistypen  
 **Boolescher Wert**  
  
## <a name="result-value"></a>Ergebniswert  
 NOT kehrt den Wert eines beliebigen booleschen Ausdrucks um.  
  
## <a name="remarks"></a>Bemerkungen  
 Durch NOT wird ein Ausdruck negiert.  
  
 Die folgende Tabelle zeigt die Ergebnisse des Vergleichs von TRUE- und FALSE-Werten mithilfe des NOT-Operators.  
  
||NICHT|  
|------|---------|  
|**TRUE**|FALSE|  
|**FALSE**|true|  
|**UNKNOWN**|UNKNOWN|  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle silberfarbenen Fahrräder gefunden, deren Standardpreis nicht über 400 € liegt.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID, Name, Color, StandardCost  
FROM Production.Product  
WHERE ProductNumber LIKE 'BK-%' AND Color = 'Silver' AND NOT StandardCost > 400;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ProductID   Name                     Color         StandardCost
 ---------   -------------------      ------      ------------
 984         Mountain-500 Silver, 40  Silver        308.2179
 985         Mountain-500 Silver, 42  Silver        308.2179
 986         Mountain-500 Silver, 44  Silver        308.2179
 987         Mountain-500 Silver, 48  Silver        308.2179
 988         Mountain-500 Silver, 52  Silver        308.2179
 (6 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Im folgenden Beispiel werden nur die Ergebnisse angezeigt, bei denen `SalesOrderNumber`-Werte mit `SO6` beginnen und `ProductKeys`-Werte größer oder gleich 400 sind.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductKey, CustomerKey, OrderDateKey, ShipDateKey  
FROM FactInternetSales  
WHERE SalesOrderNumber LIKE 'SO6%' AND NOT ProductKey < 400;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


