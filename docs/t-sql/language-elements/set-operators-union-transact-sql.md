---
description: Mengenoperatoren - UNION (Transact-SQL)
title: UNION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UNION
- UNION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UNION queries
- combining query results
- UNION operator [SQL Server]
ms.assetid: 607c296f-8a6a-49bc-975a-b8d0c0914df7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 980dfe20df5c3909e8b35a6e5b460db2568b0fe3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097087"
---
# <a name="set-operators---union-transact-sql"></a>Mengenoperatoren - UNION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Verkettet die Ergebnisse von zwei Abfragen zu einem einzelnen Resultset. Sie steuern, ob das Resultset doppelte Zeilen enthält:

- **UNION ALL**: Doppelte Zeilen sind enthalten.
- **UNION**: Doppelte Zeilen werden ausgeschlossen.

Ein **UNION**-Vorgang unterscheidet sich von einem **[JOIN](../queries/from-transact-sql.md)** -Vorgang:

- In einem **UNION**-Vorgang werden die Resultsets von zwei Abfragen verkettet. In einem **UNION**-Vorgang werden jedoch keine einzelnen Zeilen aus Spalten erstellt, die aus zwei Tabellen gesammelt wurden.
- In einem **JOIN**-Vorgang werden Spalten aus zwei Tabellen verglichen, um Ergebniszeilen zu erstellen, die aus Spalten der beiden Tabellen bestehen.
  
Für die Kombination der Resultsets von zwei Abfragen mit **UNION** gelten die folgenden Grundregeln:  
  
-   Die Anzahl und die Reihenfolge der Spalten müssen für alle Abfragen identisch sein.  
  
-   Die Datentypen müssen kompatibel sein.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ UNION [ ALL ]   
  { <query_specification> | ( <query_expression> ) } 
  [ ...n ] }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
\<query_specification> | ( \<query_expression> ) Ist eine Abfragespezifikation oder ein Abfrageausdruck, deren bzw. dessen Rückgabedaten mit den Daten aus anderen Abfragespezifikationen oder Abfrageausdrücken kombiniert werden sollen. Die Definitionen der Spalten, die Bestandteil eines UNION-Vorgangs sind, müssen nicht identisch, jedoch durch implizite Konvertierung kompatibel sein. Wenn sich die Datentypen unterscheiden, wird der Datentyp anhand der Regeln für die [Rangfolge der Datentypen](../../t-sql/data-types/data-type-precedence-transact-sql.md) bestimmt. Wenn die Typen identisch sind, sich aber in Genauigkeit, Dezimalstellen oder Länge unterscheiden, basiert das Ergebnis auf denselben Regeln wie für das Kombinieren von Ausdrücken. Weitere Informationen finden Sie unter [Genauigkeit, Dezimalstellen und Länge (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
Spalten, die dem **xml**-Datentyp angehören, müssen gleich sein. Alle Spalten müssen entweder einen XML-Schematyp aufweisen oder nicht typisiert sein. Wenn sie typisiert sein, müssen sie derselben XML-Schemaauflistung zugeordnet werden.  
  
UNION  
Gibt an, dass mehrere Resultsets kombiniert und als ein einzelnes Resultset zurückgegeben werden sollen.  
  
ALL  
Bezieht alle Zeilen inklusive Duplikate in das Ergebnis ein. Ohne diese Option werden doppelte Zeilen gelöscht.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-a-simple-union"></a>A. Verwenden einer einfachen UNION-Klausel  
Das Resultset im folgenden Beispiel enthält den Inhalt der Spalten `ProductModelID` und `Name` der Tabellen `ProductModel` und `Gloves`.  
 
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Here is the simple union.  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="b-using-select-into-with-union"></a>B. Verwenden von SELECT INTO mit UNION  
Im folgenden Beispiel gibt die `INTO`-Klausel in der zweiten `SELECT`-Anweisung an, dass die `ProductResults`-Tabelle das endgültige Resultset der Union der ausgewählten Spalten aus den Tabellen `ProductModel` und `Gloves` enthält. Die `Gloves`-Tabelle wird in der ersten `SELECT`-Anweisung erstellt.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.ProductResults', 'U') IS NOT NULL  
DROP TABLE dbo.ProductResults;  
GO  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
INTO dbo.ProductResults  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
SELECT ProductModelID, Name   
FROM dbo.ProductResults;  
```  
  
### <a name="c-using-union-of-two-select-statements-with-order-by"></a>C. Verwenden von UNION in zwei SELECT-Anweisungen mit ORDER BY  
Die Reihenfolge bestimmter Parameter, die mit der UNION-Klausel verwendet werden, ist von Bedeutung. Im folgenden Beispiel werden die ordnungsgemäße und die falsche Verwendung von `UNION` in zwei `SELECT`-Anweisungen veranschaulicht, in denen eine Spalte in der Ausgabe umbenannt werden soll.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
/* INCORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
ORDER BY Name  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
/* CORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="d-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>D: Verwenden von UNION mit drei SELECT-Anweisungen, wobei die Auswirkung von ALL und Klammern gezeigt wird  
In den folgenden Beispielen wird `UNION` verwendet, um die Ergebnisse aus drei Tabellen zu kombinieren, wobei in allen Tabellen 5 identische Datenzeilen vorhanden sind. Im ersten Beispiel wird `UNION ALL` verwendet, um doppelte Datensätze anzuzeigen, und alle 15 Zeilen zurückgegeben. Im zweiten Beispiel wird `UNION` ohne `ALL` verwendet, um die doppelten Zeilen aus den kombinierten Ergebnissen der drei `SELECT`-Anweisungen zu löschen, und 5 Zeilen zurückgegeben.  
  
Im dritten Beispiel wird `ALL` mit dem ersten `UNION` verwendet; das zweite `UNION` verwendet `ALL` nicht und ist in Klammern eingeschlossen. Da die zweite `UNION`-Anweisung in Klammern steht, wird sie zuerst verarbeitet; sie gibt 5 Zeilen zurück, weil die Option `ALL` nicht verwendet wird und Duplikate gelöscht werden. Diese 5 Zeilen werden mit den Ergebnissen der ersten `SELECT`-Anweisung mithilfe der `UNION ALL`-Schlüsselwörter kombiniert. In diesem Beispiel werden die Duplikate in den beiden aus fünf Zeilen bestehenden Resultsets nicht gelöscht. Das Endergebnis enthält 10 Zeilen.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.EmployeeOne', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeOne;  
GO  
IF OBJECT_ID ('dbo.EmployeeTwo', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeTwo;  
GO  
IF OBJECT_ID ('dbo.EmployeeThree', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeThree;  
GO  
  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeOne  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeTwo  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeThree  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
-- Union ALL  
SELECT LastName, FirstName, JobTitle  
FROM dbo.EmployeeOne  
UNION ALL  
SELECT LastName, FirstName ,JobTitle  
FROM dbo.EmployeeTwo  
UNION ALL  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle  
FROM dbo.EmployeeOne  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeOne  
UNION ALL  
(  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree  
);  
GO  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-simple-union"></a>E. Verwenden einer einfachen UNION-Klausel  
Das Resultset im folgenden Beispiel enthält den Inhalt der `CustomerKey`-Spalten der Tabellen `FactInternetSales` und `DimCustomer`. Da das ALL-Schlüsselwort nicht verwendet wird, werden Duplikate aus den Ergebnissen ausgeschlossen.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="f-using-union-of-two-select-statements-with-order-by"></a>F. Verwenden von UNION in zwei SELECT-Anweisungen mit ORDER BY  
 Wenn eine SELECT-Anweisung in einer UNION-Anweisung eine ORDER BY-Klausel enthält, muss diese Klausel nach den SELECT-Anweisungen eingefügt werden. Im folgenden Beispiel werden die ordnungsgemäße und die falsche Verwendung von `UNION` in zwei `SELECT`-Anweisungen veranschaulicht, in denen eine Spalte mit ORDER BY sortiert wird.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT  
SELECT CustomerKey   
FROM FactInternetSales    
ORDER BY CustomerKey  
UNION   
SELECT CustomerKey   
FROM DimCustomer  
ORDER BY CustomerKey;  
  
-- CORRECT   
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="g-using-union-of-two-select-statements-with-where-and-order-by"></a>G. Verwenden von UNION in zwei SELECT-Anweisungen mit WHERE und ORDER BY  
Im folgenden Beispiel werden die ordnungsgemäße und die falsche Verwendung von `UNION` in zwei `SELECT`-Anweisungen veranschaulicht, in denen WHERE und ORDER BY benötigt wird.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT   
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
ORDER BY CustomerKey   
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
  
-- CORRECT  
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="h-using-union-of-three-select-statements-to-show-effects-of-all-and-parentheses"></a>H. Verwenden von UNION mit drei SELECT-Anweisungen, wobei die Auswirkung von ALL und Klammern gezeigt wird  
In den folgenden Beispielen wird `UNION` zum Kombinieren der Ergebnisse **einer Tabelle** verwendet, um die Auswirkungen von ALL und Klammern bei Verwendung von `UNION` zu zeigen.  
  
Im ersten Beispiel wird `UNION ALL` verwendet, um doppelte Datensätze anzuzeigen. Zudem wird jede Zeile in der Quelltabelle dreimal zurückgegeben. Im zweiten Beispiel wird `UNION` ohne `ALL` verwendet, um die doppelten Zeilen aus den kombinierten Ergebnissen der drei `SELECT`-Anweisungen zu löschen, und nur die nicht doppelten Zeilen aus der Quelltabelle zurückgegeben.  
  
Im dritten Beispiel wird `ALL` mit dem ersten `UNION` verwendet; das zweite `UNION` verwendet `ALL` nicht und ist in Klammern eingeschlossen. Das zweite `UNION` wird zuerst verarbeitet, da es in Klammern angegeben ist. Es werden nur die nicht doppelten Zeilen aus der Tabelle zurückgegeben, da die `ALL`-Option nicht verwendet wird und Duplikate entfernt werden. Diese Zeilen werden mit den Ergebnissen der ersten `SELECT`-Anweisung mithilfe der `UNION ALL`-Schlüsselwörter kombiniert. In diesem Beispiel werden die Duplikate in den beiden Resultsets nicht gelöscht.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL  
(  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[SELECT-Beispiele (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
