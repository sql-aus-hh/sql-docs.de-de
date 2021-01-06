---
description: Erstellen von benutzerdefinierten Funktionen (Datenbank-Engine)
title: Erstellen von benutzerdefinierten Funktionen (Datenbank-Engine) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SCHEMABINDING clause
- schema-bound functions [SQL Server]
- user-defined functions [SQL Server], creating
- CREATE FUNCTION statement
- valid statements [SQL Server]
- UDF
- TVF
ms.assetid: f0d5dd10-73fd-4e05-9177-07f56552bdf7
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3303df6ac71b7a6c92d4a1c65a8b7b4b3229cd6e
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743677"
---
# <a name="create-user-defined-functions-database-engine"></a>Erstellen von benutzerdefinierten Funktionen (Datenbank-Engine)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  In diesem Thema wird beschrieben, wie eine benutzerdefinierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktion (User-defined Function, UDF) mit [!INCLUDE[tsql](../../includes/tsql-md.md)]erstellt wird.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Mit benutzerdefinierten Funktionen können keine Aktionen ausgeführt werden, die den Status einer Datenbank ändern.  
  
-   Benutzerdefinierte Funktionen dürfen keine `OUTPUT INTO`-Klausel enthalten, deren Ziel eine Tabelle ist.  
  
-   Von benutzerdefinierten Funktionen können nicht mehrere Resultsets zurückgegeben werden. Falls mehrere Resultsets zurückgegeben werden müssen, verwenden Sie eine gespeicherte Prozedur.  
  
-   Die Fehlerbehandlung ist in einer benutzerdefinierten Funktion eingeschränkt. Eine benutzerdefinierte Funktion bietet keine Unterstützung für `TRY...CATCH`, `@ERROR` oder `RAISERROR`.  
  
-   Von benutzerdefinierten Funktionen kann zwar keine gespeicherte Prozedur, aber eine erweiterte gespeicherte Prozedur aufgerufen werden.  
  
-   Von benutzerdefinierten Funktionen können kein dynamisches SQL bzw. keine temporären Tabellen verwendet werden. Tabellenvariablen sind zulässig.  
  
-   `SET`-Anweisungen sind in einer benutzerdefinierten Funktion nicht zulässig.  
  
-   Die `FOR XML`-Klausel ist nicht zulässig.  
  
-   Benutzerdefinierte Funktionen können geschachtelt werden. Dies bedeutet, dass eine benutzerdefinierte Funktion eine andere aufrufen kann. Die Schachtelungsebene wird um eins erhöht, wenn die aufgerufene Funktion mit der Ausführung beginnt, und wird wieder um eins erniedrigt, wenn die aufgerufene Funktion die Ausführung beendet. Benutzerdefinierte Funktionen unterstützen bis zu 32 geschachtelte Ebenen. Ein Überschreiten der maximalen Schachtelungsebenen verursacht das Fehlschlagen der gesamten Funktionsaufrufskette. Alle Verweise auf verwalteten Code von einer benutzerdefinierten Transact-SQL-Funktion aus gelten hinsichtlich des Maximums von 32 Schachtelungsebenen als eine Ebene. Methoden, die aus verwaltetem Code aufgerufen werden, werden nicht mitgezählt.  
  
-   Die folgenden Service Broker-Anweisungen **können nicht** in die Definition einer benutzerdefinierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion aufgenommen werden:  
  
    -   `BEGIN DIALOG CONVERSATION`  
  
    -   `END CONVERSATION`  
  
    -   `GET CONVERSATION GROUP`  
  
    -   `MOVE CONVERSATION`  
  
    -   `RECEIVE`  
  
    -   `SEND`  
  
###  <a name="permissions"></a><a name="Security"></a> Berechtigungen 

Erfordert die `CREATE FUNCTION`-Berechtigung in der Datenbank und die `ALTER`-Berechtigung für das Schema, in dem die Funktion erstellt wird. Wenn die Funktion einen benutzerdefinierten Typ angibt, wird die `EXECUTE`-Berechtigung für den Typ benötigt.  
  
##  <a name="scalar-functions"></a><a name="Scalar"></a> Skalarfunktionen  
 Im folgenden Beispiel wird eine **Skalarfunktion (benutzerdefinierte Skalarfunktion)** mit mehreren Anweisungen in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt. Die Funktion nimmt einen Eingabewert ( `ProductID`) an und gibt einen einzelnen Datenwert zurück, der die aggregierte Menge des Lagerbestands für das angegebene Produkt darstellt.  
  
```sql  
IF OBJECT_ID (N'dbo.ufnGetInventoryStock', N'FN') IS NOT NULL  
    DROP FUNCTION ufnGetInventoryStock;  
GO  
CREATE FUNCTION dbo.ufnGetInventoryStock(@ProductID int)  
RETURNS int   
AS   
-- Returns the stock level for the product.  
BEGIN  
    DECLARE @ret int;  
    SELECT @ret = SUM(p.Quantity)   
    FROM Production.ProductInventory p   
    WHERE p.ProductID = @ProductID   
        AND p.LocationID = '6';  
     IF (@ret IS NULL)   
        SET @ret = 0;  
    RETURN @ret;  
END; 
```  
  
 Im folgenden Beispiel wird die `ufnGetInventoryStock` -Funktion verwendet, um den aktuellen Lagerbestand für Produkte mit einer `ProductModelID` zwischen 75 und 80 zurückzugeben.  
  
```sql  
SELECT ProductModelID, Name, dbo.ufnGetInventoryStock(ProductID)AS CurrentSupply  
FROM Production.Product  
WHERE ProductModelID BETWEEN 75 and 80;  
```  

> [!NOTE]  
> Weitere Informationen und Beispiele für Skalarfunktionen finden Sie unter [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

##  <a name="table-valued-functions"></a><a name="TVF"></a> Tabellenwertfunktionen  
Im folgenden Beispiel wird eine **Inline-Tabellenwertfunktion** in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt. Die Funktion nimmt einen Eingabeparameter (eine Kunden-ID (Geschäfts-ID)) an und gibt die Spalten `ProductID`, `Name`sowie das Aggregat der bisherigen Verkaufseinnahmen dieses Jahres als `YTD Total` für jedes Produkt zurück, das an das Geschäft verkauft wurde.  
  
```sql  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
```  
  
Das folgende Beispiel ruft die Funktion auf und gibt die Kunden-ID 602 an.  
  
```sql  
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
Im folgenden Beispiel wird eine **Tabellenwertfunktion mit mehreren Anweisungen** in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt. Die Funktion nimmt einen einzelnen Eingabeparameter ( `EmployeeID` ) an und gibt eine Liste aller Mitarbeiter zurück, die dem angegebenen Mitarbeiter direkt oder indirekt unterstellt sind. Die Funktion wird dann unter Angabe der Mitarbeiternummer 109 aufgerufen.  
  
```sql  
IF OBJECT_ID (N'dbo.ufn_FindReports', N'TF') IS NOT NULL  
    DROP FUNCTION dbo.ufn_FindReports;  
GO  
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)  
RETURNS @retFindReports TABLE   
(  
    EmployeeID int primary key NOT NULL,  
    FirstName nvarchar(255) NOT NULL,  
    LastName nvarchar(255) NOT NULL,  
    JobTitle nvarchar(50) NOT NULL,  
    RecursionLevel int NOT NULL  
)  
--Returns a result set that lists all the employees who report to the   
--specific employee directly or indirectly.*/  
AS  
BEGIN  
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns  
    AS (  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0 -- Get the initial list of Employees for Manager n  
        FROM HumanResources.Employee e   
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        WHERE e.BusinessEntityID = @InEmpID  
        UNION ALL  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1 -- Join recursive member to anchor  
        FROM HumanResources.Employee e   
            INNER JOIN EMP_cte  
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode  
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        )  
-- copy the required columns to the result of the function   
   INSERT @retFindReports  
   SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
   FROM EMP_cte   
   RETURN  
END;  
GO  
```  
  
Das folgende Beispiel ruft die Funktion auf und gibt die Mitarbeiternummer 1 an.  
  
```sql  
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
FROM dbo.ufn_FindReports(1);  
```  

> [!NOTE]  
> Weitere Informationen und Beispiele für Inline-Tabellenwertfunktionen und Tabellenwertfunktionen mit mehreren Anweisungen finden Sie unter [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

## <a name="best-practices"></a>Bewährte Methoden  
Wenn eine benutzerdefinierte Funktion nicht mit der `SCHEMABINDING`-Klausel erstellt wurde, können sich die an zugrunde liegenden Objekten vorgenommenen Änderungen auf die Definition der Funktion auswirken und bei Aufruf der Funktion zu unerwarteten Ergebnissen führen. Es wird empfohlen, eine der folgenden Methoden zu implementieren, damit die Funktion aufgrund von Änderungen an den zugrunde liegenden Objekten nicht veraltet ist:  
  
-   Geben Sie beim Erstellen der benutzerdefinierten Funktion die `WITH SCHEMABINDING`-Klausel an. Hiermit wird sichergestellt, dass die Objekte, auf die in der Funktionsdefinition verwiesen wird, nicht geändert werden können, es sei denn, die Funktion wird auch geändert.  
  
-   Führen Sie die gespeicherte Prozedur [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) aus, nachdem Sie ein Objekt geändert haben, das in der Definition der benutzerdefinierten Funktion angegeben ist.  

Wenn Sie eine benutzerdefinierte Funktion erstellen, die nicht auf Daten zugreift, geben Sie die Option `SCHEMABINDING` an. Dadurch wird verhindert, dass der Abfrageoptimierer unnötige Spool-Operatoren für Abfragepläne generiert, die diese benutzerdefinierten Funktionen enthalten. Weitere Informationen zu Spoolvorgängen finden Sie unter [Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md). Weitere Informationen zum Erstellen einer schemagebundenen Funktion finden Sie unter [Schemagebundene Funktionen](../../relational-databases/user-defined-functions/user-defined-functions.md#SchemaBound).

Ein Verknüpfen mit einer Tabellenwertfunktion mit mehreren Anweisungen in einer `FROM`-Klausel ist möglich, kann jedoch zu Leistungseinbußen führen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann für einige Anweisungen, die in einer Tabellenwertfunktion mit mehreren Anweisungen enthalten sein können, nicht alle optimierten Techniken verwenden, was zu einem suboptimalen Abfrageplan führt. Um die bestmögliche Leistung zu erzielen, sollten nach Möglichkeit anstelle von Funktionen Joins zwischen Basistabellen verwendet werden.  

> [!IMPORTANT]
> Die festgelegte Kardinalitätsschätzung von Tabellenwertfunktionen mit mehreren Anweisungen beträgt ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] „100“ und in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] „1“.    
> Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] kann beim Optimieren eines Ausführungsplans, der Tabellenwertfunktionen mit mehreren Anweisungen verwendet, eine verschachtelte Ausführung genutzt werden, was dazu führt, dass die tatsächliche Kardinalität anstelle der oben genannten Heuristik verwendet wird.     
> Weitere Informationen finden Sie unter [Verschachtelte Ausführung mit Tabellenwertfunktionen mit mehreren Anweisungen](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).

> [!NOTE]  
> ANSI_WARNINGS wird bei der Übergabe von Parametern in einer gespeicherten Prozedur oder in einer benutzerdefinierten Funktion oder beim Deklarieren und Festlegen von Variablen in einer Batchanweisung nicht berücksichtigt. Wird beispielsweise eine Variable als **char(3)** definiert und dann auf einen Wert festgelegt, der länger als drei Zeichen ist, werden die Daten auf die definierte Größe abgeschnitten, und die Anweisung `INSERT` oder `UPDATE` wird erfolgreich ausgeführt.

## <a name="see-also"></a>Weitere Informationen  
 [Benutzerdefinierte Funktionen](../../relational-databases/user-defined-functions/user-defined-functions.md)     
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)    
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)    
 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)     
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)    
 Weitere Beispiele in der [Community](https://www.bing.com/search?q=user%20defined%20function%20%22sql%20server%202016%22%20examples&qs=n&form=QBRE&pq=user%20defined%20function%20%22sql%20server%202016%22%20examples&sc=0-48&sp=-1&sk=&cvid=C3AD337125A840AD9EEFA3AAC36A3712)   
  
