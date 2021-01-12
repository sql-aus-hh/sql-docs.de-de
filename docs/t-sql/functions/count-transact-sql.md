---
description: COUNT (Transact-SQL)
title: COUNT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COUNT_TSQL
- COUNT
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT function
- totals [SQL Server]
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT function [Transact-SQL]
ms.assetid: 28d39da6-bc2e-46c7-858c-b1721c938830
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ac784713b7a74eb08acdfa9478b519c0b80940c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101289"
---
# <a name="count-transact-sql"></a>COUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt die Anzahl der in einer Gruppe gefundenen Elemente zurück. `COUNT` arbeitet wie die [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md)-Funktion. Diese Funktionen unterscheiden sich nur in den Datentypen ihrer Rückgabewerte. `COUNT` gibt immer einen Wert vom Datentyp **int** zurück. `COUNT_BIG` gibt immer einen Wert vom Datentyp **bigint** zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT ( { [ [ ALL | DISTINCT ] expression ] | * } )  

-- Analytic Function Syntax  
COUNT ( [ ALL ]  { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
**ALL**  
Wendet die Aggregatfunktion auf alle Werte an. ALL dient als Standardeinstellung.
  
DISTINCT  
Gibt an, dass `COUNT` die Anzahl der eindeutigen Werte zurückgibt, die nicht NULL sind.
  
*expression*  
Eine [expression](../../t-sql/language-elements/expressions-transact-sql.md) beliebigen Typs mit Ausnahme von **image**, **ntext** oder **text**. Beachten Sie, dass `COUNT` keine Aggregatfunktionen oder Unterabfragen in einem Ausdruck unterstützt.
  
\*  
Gibt an, dass `COUNT` alle Zeilen zählen soll, um die Gesamtzahl der zurückzugebenden Tabellenzeilen zu bestimmen. `COUNT(*)` nimmt keine Parameter an und unterstützt die Verwendung von DISTINCT nicht. `COUNT(*)` erfordert keinen *expression*-Parameter, da definitionsgemäß keine Informationen zu einer bestimmten Spalte verwendet werden. `COUNT(*)` gibt die Anzahl der Zeilen in einer angegebenen Tabelle zurück. Duplikate werden beibehalten. Dabei werden alle Zeilen einzeln gezählt. Dies schließt Zeilen mit NULL-Werten ein.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] [ *ROW_or_RANGE_clause* ] **)**  
Das Argument *partition_by_clause* unterteilt das von der `FROM`-Klausel erzeugte Resultset in Partitionen, auf die die `COUNT`-Funktion angewendet wird. Wird dies nicht angegeben, verarbeitet die Funktion alle Zeilen des Abfrageresultsets als einzelne Gruppe. *order_by_clause* bestimmt die logische Reihenfolge, in der der Vorgang ausgeführt wird. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md). 

## <a name="return-types"></a>Rückgabetypen
 **int**  
  
## <a name="remarks"></a>Bemerkungen  
COUNT(\*) gibt die Anzahl der Elemente in einer Gruppe zurück. Dies schließt NULL-Werte und Duplikate ein.
  
COUNT(ALL *expression*) wertet *expression* für jede Zeile in einer Gruppe aus und gibt die Anzahl der Werte zurück, die nicht NULL sind.
  
COUNT(DISTINCT *expression*) wertet *expression* für jede Zeile in einer Gruppe aus und gibt die Anzahl der eindeutigen Werte zurück, die nicht NULL sind.
  
Für Rückgabewerte größer als 2^31-1 gibt `COUNT` einen Fehler zurück. Verwenden Sie für diese Fälle stattdessen `COUNT_BIG`.
  
`COUNT` ist eine deterministische Funktion, wenn sie ***ohne** _ die OVER- und ORDER BY-Klauseln verwendet wird. Sie ist nicht deterministisch, wenn sie _*_mit_*_ den OVER- und ORDER BY-Klauseln verwendet wird. Weitere Informationen finden Sie unter [Deterministische und nicht deterministische Funktionen](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-count-and-distinct"></a>A. Verwenden von COUNT und DISTINCT  
Dieses Beispiel gibt die Anzahl der verschiedenen Positionen zurück, die ein [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]-Mitarbeiter innehaben kann.
  
```sql
SELECT COUNT(DISTINCT Title)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67  
  
(1 row(s) affected)
```
  
### <a name="b-using-count_"></a>B. Unter Verwendung von COUNT(\_)  
Dieses Beispiel gibt die Gesamtzahl der [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]-Mitarbeiter zurück.
  
```sql
SELECT COUNT(*)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
290  
  
(1 row(s) affected)
```
  
### <a name="c-using-count-with-other-aggregates"></a>C. Unter Verwendung von COUNT(\*) mit weiteren Aggregaten  
Dieses Beispiel zeigt, dass `COUNT(*)` mit anderen Aggregatfunktionen in der `SELECT`-Liste funktioniert. Im Beispiel wird die [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank verwendet.
  
```sql
SELECT COUNT(*), AVG(Bonus)  
FROM Sales.SalesPerson  
WHERE SalesQuota > 25000;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- ---------------------
14            3472.1428
  
(1 row(s) affected)
```
  
### <a name="d-using-the-over-clause"></a>D. Verwenden der OVER-Klausel  
In diesem Beispiel werden die Funktionen `MIN`, `MAX`, `AVG` und `COUNT` zusammen mit der `OVER`-Klausel verwendet, um aggregierte Werte für jede Abteilung in der `HumanResources.Department`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückzugeben.
  
```sql
SELECT DISTINCT Name  
       , MIN(Rate) OVER (PARTITION BY edh.DepartmentID) AS MinSalary  
       , MAX(Rate) OVER (PARTITION BY edh.DepartmentID) AS MaxSalary  
       , AVG(Rate) OVER (PARTITION BY edh.DepartmentID) AS AvgSalary  
       ,COUNT(edh.BusinessEntityID) OVER (PARTITION BY edh.DepartmentID) AS EmployeesPerDept  
FROM HumanResources.EmployeePayHistory AS eph  
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
     ON eph.BusinessEntityID = edh.BusinessEntityID  
JOIN HumanResources.Department AS d  
ON d.DepartmentID = edh.DepartmentID
WHERE edh.EndDate IS NULL  
ORDER BY Name;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Name                          MinSalary             MaxSalary             AvgSalary             EmployeesPerDept  
----------------------------- --------------------- --------------------- --------------------- ----------------  
Document Control              10.25                 17.7885               14.3884               5  
Engineering                   32.6923               63.4615               40.1442               6  
Executive                     39.06                 125.50                68.3034               4  
Facilities and Maintenance    9.25                  24.0385               13.0316               7  
Finance                       13.4615               43.2692               23.935                10  
Human Resources               13.9423               27.1394               18.0248               6  
Information Services          27.4038               50.4808               34.1586               10  
Marketing                     13.4615               37.50                 18.4318               11  
Production                    6.50                  84.1346               13.5537               195  
Production Control            8.62                  24.5192               16.7746               8  
Purchasing                    9.86                  30.00                 18.0202               14  
Quality Assurance             10.5769               28.8462               15.4647               6  
Research and Development      40.8654               50.4808               43.6731               4  
Sales                         23.0769               72.1154               29.9719               18  
Shipping and Receiving        9.00                  19.2308               10.8718               6  
Tool Design                   8.62                  29.8462               23.5054               6  
  
(16 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-count-and-distinct"></a>E. Verwenden von COUNT und DISTINCT  
Dieses Beispiel gibt die Anzahl der verschiedenen Positionen zurück, die ein Mitarbeiter eines bestimmten Unternehmens innehaben kann.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(DISTINCT Title)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67
```  
  
### <a name="f-using-count"></a>F. Unter Verwendung von COUNT(\*)  
In diesem Beispiel wird die Gesamtzahl der Zeilen in der `dbo.DimEmployee`-Tabelle zurückgegeben.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(*)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-------------
296
```  
  
### <a name="g-using-count-with-other-aggregates"></a>G. Unter Verwendung von COUNT(\*) mit weiteren Aggregaten  
Dieses Beispiel vereint `COUNT(*)` mit weiteren Aggregatfunktionen in der `SELECT`-Liste. Es gibt die Anzahl der Vertriebsmitarbeiter mit einer jährlichen Sollvorgabe für den Verkauf von über 500.000 USD und die durchschnittliche Sollvorgabe dieser Vertriebsmitarbeiter für den Verkauf zurück.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(EmployeeKey) AS TotalCount, AVG(SalesAmountQuota) AS [Average Sales Quota]  
FROM dbo.FactSalesQuota  
WHERE SalesAmountQuota > 500000 AND CalendarYear = 2001;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
TotalCount  Average Sales Quota
----------  -------------------
10          683800.0000
```
  
### <a name="h-using-count-with-having"></a>H. Verwenden von COUNT mit HAVING  
Dieses Beispiel verwendet `COUNT` mit der `HAVING`-Klausel, um die Abteilungen einer Firma zurückzugeben, von denen jede mehr als 15 Mitarbeiter aufweist.
  
```sql
USE ssawPDW;  
  
SELECT DepartmentName,   
       COUNT(EmployeeKey)AS EmployeesInDept  
FROM dbo.DimEmployee  
GROUP BY DepartmentName  
HAVING COUNT(EmployeeKey) > 15;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
DepartmentName  EmployeesInDept
--------------  ---------------
Sales           18
Production      179
```
  
### <a name="i-using-count-with-over"></a>I. Verwenden von COUNT mit OVER  
In diesem Beispiel wird `COUNT` mit der `OVER`-Klausel verwendet, um die Anzahl der enthaltenen Produkte für jeden der angegebenen Verkaufsaufträge zurückzugeben.
  
```sql
USE ssawPDW;  
  
SELECT DISTINCT COUNT(ProductKey) OVER(PARTITION BY SalesOrderNumber) AS ProductCount  
    ,SalesOrderNumber  
FROM dbo.FactInternetSales  
WHERE SalesOrderNumber IN (N'SO53115',N'SO55981');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
ProductCount   SalesOrderID`
------------   -----------------
3              SO53115
1              SO55981
```
  
## <a name="see-also"></a>Weitere Informationen
[Aggregate Functions &#40;Transact-SQL&#41; (Aggregatfunktionen (Transact-SQL))](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/count-big-transact-sql.md)  
[OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  


