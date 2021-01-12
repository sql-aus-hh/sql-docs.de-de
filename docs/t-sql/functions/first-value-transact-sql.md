---
description: FIRST_VALUE (Transact-SQL)
title: FIRST_VALUE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FIRST_VALUE_TSQL
- FIRST_VALUE
dev_langs:
- TSQL
helpviewer_keywords:
- FIRST_VALUE function
- analytic functions, FIRST_VALUE
ms.assetid: 1990c3c7-dad2-48db-b2cd-3e8bd2c49d17
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e8c38ffc45933da64b9921a035e956d337f4e1f4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101181"
---
# <a name="first_value-transact-sql"></a>FIRST_VALUE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Hiermit wird der erste Wert in einer geordneten Menge von Werten zurückgegeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
FIRST_VALUE ( [scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause [ rows_range_clause ] )
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *scalar_expression*  
 Der zurückzugebende Wert. *scalar_expression* kann eine Spalte, eine Unterabfrage oder ein anderer willkürlicher Ausdruck sein, durch die sich ein einzelner Wert ergibt. Andere analytische Funktionen sind nicht zulässig.  

 [ IGNORE NULLS | RESPECT NULLS ]     
 **Gilt für**: Azure SQL Edge

 IGNORE NULLS: Hiermit werden NULL-Werte im Dataset beim Berechnen des ersten Werts einer Partition ignoriert.     
 RESPECT NULLS: Hiermit werden NULL-Werte im Dataset beim Berechnen des ersten Werts einer Partition berücksichtigt.     
 
  Weitere Informationen finden Sie unter [Eingeben fehlender Werte](/azure/azure-sql-edge/imputing-missing-values/).
  
 OVER **(** [ *partition_by_clause* ] *order_by_clause* [ *rows_range_clause* ] **)**  
 *partition_by_clause* unterteilt das von der FROM-Klausel erzeugte Resultset in Partitionen, auf die die Funktion angewendet wird. Wird dies nicht angegeben, verarbeitet die Funktion alle Zeilen des Abfrageresultsets als einzelne Gruppe. *order_by_clause* bestimmt die logische Reihenfolge, in der der Vorgang ausgeführt wird. *order_by_clause* ist erforderlich. *rows_range_clause* schränkt die Zeilen innerhalb der Partition weiter ein, indem Ausgangs- und Endpunkte angegeben werden. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Rückgabetypen  
 Entspricht dem Typ von *scalar_expression*.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 FIRST_VALUE ist nicht deterministisch. Weitere Informationen finden Sie unter [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-first_value-over-a-query-result-set"></a>A. Verwenden von FIRST_VALUE für ein Abfrageresultset  
 Im folgenden Beispiel wird der Name des günstigsten Produkts in einer bestimmten Produktkategorie mithilfe von FIRST_VALUE zurückgegeben.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, ListPrice,   
       FIRST_VALUE(Name) OVER (ORDER BY ListPrice ASC) AS LeastExpensive   
FROM Production.Product  
WHERE ProductSubcategoryID = 37;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Name                    ListPrice             LeastExpensive  
----------------------- --------------------- --------------------  
Patch Kit/8 Patches     2.29                  Patch Kit/8 Patches  
Road Tire Tube          3.99                  Patch Kit/8 Patches  
Touring Tire Tube       4.99                  Patch Kit/8 Patches  
Mountain Tire Tube      4.99                  Patch Kit/8 Patches  
LL Road Tire            21.49                 Patch Kit/8 Patches  
ML Road Tire            24.99                 Patch Kit/8 Patches  
LL Mountain Tire        24.99                 Patch Kit/8 Patches  
Touring Tire            28.99                 Patch Kit/8 Patches  
ML Mountain Tire        29.99                 Patch Kit/8 Patches  
HL Road Tire            32.60                 Patch Kit/8 Patches  
HL Mountain Tire        35.00                 Patch Kit/8 Patches  
  
```  
  
### <a name="b-using-first_value-over-partitions"></a>B. Verwenden von FIRST_VALUE über Partitionen  
 Im folgenden Beispiel werden die Mitarbeiter mit der geringsten Anzahl von Urlaubsstunden im Vergleich zu anderen Angestellten mit der gleichen Berufsbezeichnung mithilfe von FIRST_VALUE zurückgegeben. Die PARTITION BY-Klausel partitioniert die Mitarbeiter nach Berufsbezeichnung, und die FIRST_VALUE-Funktion wird unabhängig auf jede Partition angewendet. Die in der OVER-Klausel angegebene ORDER BY-Klausel bestimmt die logische Reihenfolge, in der die FIRST_VALUE-Funktion auf die Zeilen in jeder Partition angewendet wird. Die ROWS UNBOUNDED PRECEDING-Klausel gibt den Ausgangspunkt des Fensters als erste Zeile jeder Partition an.  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT JobTitle, LastName, VacationHours,   
       FIRST_VALUE(LastName) OVER (PARTITION BY JobTitle   
                                   ORDER BY VacationHours ASC  
                                   ROWS UNBOUNDED PRECEDING  
                                  ) AS FewestVacationHours  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p   
    ON e.BusinessEntityID = p.BusinessEntityID  
ORDER BY JobTitle;  
```  
  
 Dies ist ein Auszug aus dem Resultset.  
  
```  
  
JobTitle                            LastName                  VacationHours FewestVacationHours  
----------------------------------- ------------------------- ------------- -------------------  
Accountant                          Moreland                  58            Moreland  
Accountant                          Seamans                   59            Moreland  
Accounts Manager                    Liu                       57            Liu  
Accounts Payable Specialist         Tomic                     63            Tomic  
Accounts Payable Specialist         Sheperdigian              64            Tomic  
Accounts Receivable Specialist      Poe                       60            Poe  
Accounts Receivable Specialist      Spoon                     61            Poe  
Accounts Receivable Specialist      Walton                    62            Poe  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)  
 [Last_Value &#40;Transact-SQL&#41;](last-value-transact-sql.md)  

