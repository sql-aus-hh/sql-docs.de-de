---
description: SELECT - ORDER BY-Klausel (Transact-SQL)
title: ORDER BY-Klausel (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ORDER_TSQL
- BY
- ORDER_BY_TSQL
- BY_TSQL
- ORDER
- ORDER BY
dev_langs:
- TSQL
helpviewer_keywords:
- ad-hoc query paging
- OFFSET clause
- SELECT statement [SQL Server], FETCH clause
- clauses [SQL Server], ORDER BY
- SELECT statement [SQL Server], limiting the rows returned
- data [SQL Server], limiting the rows returned
- data [SQL Server], ad-hoc query paging
- sort orders [SQL Server]
- SELECT statement [SQL Server], OFFSET clause
- ORDER BY clause [Transact-SQL]
- LIMIT
- limiting result sets
- SELECT statement [SQL Server], ORDER BY clause
- query paging
- data [SQL Server], sorting
- limiting data returned in a query
- sort orders [SQL Server], ORDER BY clause
- FETCH clause
ms.assetid: bb394abe-cae6-4905-b5c6-8daaded77742
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7903d3a208bd08c9ba22d251a0e4124e81725ea
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711986"
---
# <a name="select---order-by-clause-transact-sql"></a>SELECT - ORDER BY-Klausel (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Sortiert von einer Abfrage in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurückgegebene Daten. Verwenden Sie diese Klausel wie folgt:  
  
-   Sortieren Sie das Resultset einer Abfrage anhand der angegebenen Spaltenliste und schränken Sie optional die für einen angegebenen Bereich zurückgegebenen Zeilen ein. Die Reihenfolge, in der Zeilen in einem Resultset zurückgegeben werden, ist nicht garantiert, es sei denn, eine ORDER BY-Klausel wird angegeben.  
  
-   Bestimmen Sie die Reihenfolge, in der Werte der [Rangfolgenfunktion](../../t-sql/functions/ranking-functions-transact-sql.md) auf das Resultset angewendet werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  ORDER BY wird in SELECT/INTO- oder CREATE TABLE AS SELECT (CTAS)-Anweisungen in [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] oder [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] nicht unterstützt.

## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]   
[ <offset_fetch> ]  
  
<offset_fetch> ::=  
{   
    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }  
    [  
      FETCH { FIRST | NEXT } {integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY  
    ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ ORDER BY   
    {  
    order_by_expression   
    [ ASC | DESC ]   
    } [ ,...n ]   
]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *order_by_expression*  
 Gibt eine Spalte oder einen Ausdruck an, anhand derer das Abfrageresultset sortiert werden soll. Eine Sortierspalte kann als Name, Spaltenalias oder eine nicht negative ganze Zahl angegeben werden, die die Position der Spalte in der Auswahlliste darstellt.  
  
 Es können mehrere Sortierspalten angegeben werden. Spaltennamen müssen eindeutig sein. Die Reihenfolge der Sortierspalten in der ORDER BY-Klausel definiert die Anordnung des sortierten Resultsets. Dies bedeutet, dass das Resultset anhand der ersten Spalte sortiert wird, und diese sortierte Liste wird anhand der zweiten Spalte sortiert usw.  
  
 Die Spaltennamen, auf die in der ORDER BY-Klausel verwiesen wird, müssen entweder einer Spalte oder einem Spaltenalias in der Auswahlliste oder einer Spalte aus einer Tabelle in der FROM-Klausel entsprechen, ohne dass dabei Zweideutigkeiten zulässig sind. Wenn die ORDER BY-Klausel auf einen Spaltenalias aus der Auswahlliste verweist, muss der Spaltenalias eigenständig und nicht als Teil eines Ausdrucks in der ORDER BY-Klausel verwendet werden, zum Beispiel folgendermaßen:
 
```sql
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName; -- correct 
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName + ''; -- wrong
```
  
 COLLATE *collation_name*  
 Gibt an, dass der ORDER BY-Vorgang gemäß der in *collation_name* angegebenen Sortierung und nicht gemäß der in der Tabelle oder Sicht definierten Sortierung der Spalte ausgeführt werden soll. *collation_name* kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname sein. Weitere Informationen finden Sie unter [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). COLLATE ist nur für Spalten vom Typ **char**, **varchar**, **nchar** und **nvarchar** anwendbar.  
  
 **ASC** | DESC  
 Gibt an, dass die Werte in der angegebenen Spalte in aufsteigender oder absteigender Reihenfolge sortiert werden sollen. ASC sortiert vom niedrigsten Wert zum höchsten Wert. DESC sortiert vom höchsten Wert zum niedrigsten Wert. ASC ist die Standardsortierreihenfolge. NULL-Werte werden als die niedrigsten Werte behandelt, die möglich sind.  
  
 OFFSET { *integer_constant* | *offset_row_count_expression* } { ROW | ROWS }  
 Gibt die Anzahl der Zeilen an, die übersprungen werden soll, bevor Zeilen vom Abfrageausdruck zurückgegeben werden. Der Wert kann eine ganzzahlige Konstante oder ein Ausdruck größer oder gleich 0 sein.  
  
**Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *offset_row_count_expression* kann eine Variable, ein Parameter oder eine konstante skalare Unterabfrage sein. Bei einer Unterabfrage sind keine Verweise auf Spalten möglich, die im äußeren Abfragebereich definiert wurden. Dies bedeutet, das keine Korrelation mit der äußeren Abfrage möglich ist.  
  
 ROW und ROWS sind Synonyme und werden mit ANSI-Kompatibilität bereitgestellt.  
  
 In Abfrageausführungsplänen wird der Wert für die Offsetzeilenanzahl im **Offset**-Attribut des TOP-Abfrageoperators angezeigt.  
  
 FETCH { FIRST \| NEXT } { *integer_constant* \| *fetch_row_count_expression* } { ROW \| ROWS } ONLY  
 Gibt die Anzahl der Zeilen an, die zurückgegeben werden sollen, nachdem die OFFSET-Klausel verarbeitet wurde. Der Wert kann eine ganzzahlige Konstante oder ein Ausdruck größer oder gleich 1 sein.  
  
**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *fetch_row_count_expression* kann eine Variable, ein Parameter oder eine konstante skalare Unterabfrage sein. Bei einer Unterabfrage sind keine Verweise auf Spalten möglich, die im äußeren Abfragebereich definiert wurden. Dies bedeutet, das keine Korrelation mit der äußeren Abfrage möglich ist.  
  
 FIRST und NEXT sind Synonyme und werden mit ANSI-Kompatibilität bereitgestellt.  
  
 ROW und ROWS sind Synonyme und werden mit ANSI-Kompatibilität bereitgestellt.  
  
 In Abfrageausführungsplänen wird der Wert für die Offsetzeilenanzahl im Attribut **Rows** oder **Top** des TOP-Abfrageoperators angezeigt.  
  
## <a name="best-practices"></a>Bewährte Methoden  
 Geben Sie in der ORDER BY-Klausel keine ganzen Zahlen als Positionsangaben der Spalten in der Auswahlliste an. Auch wenn eine Anweisung wie `SELECT ProductID, Name FROM Production.Production ORDER BY 2` nicht ungültig ist, wird dadurch im Vergleich zur Angabe des tatsächlichen Spaltennamens das Verständnis durch andere erschwert. Außerdem erfordern Änderungen an der Auswahlliste, etwa eine Änderung der Spaltenreihenfolge oder das Hinzufügen neuer Spalten, auch Änderungen an der ORDER BY-Klausel, um unerwartete Ergebnisse zu vermeiden.  
  
 Verwenden Sie in einer SELECT TOP (*N*)-Anweisung immer eine ORDER BY-Klausel. Dies ist die einzige Möglichkeit, zuverlässig anzugeben, welche Zeilen von TOP betroffen sind. Weitere Informationen finden Sie unter [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
## <a name="interoperability"></a>Interoperabilität  
 Wenn sie mit einer SELECT...INTO oder INSERT...SELECT-Anweisung zum Einfügen von Zeilen aus einer anderen Quelle verwendet wird, garantiert die ORDER BY-Klausel nicht, dass die Zeilen in der angegebenen Reihenfolge eingefügt werden.  
  
 Die Verwendung von OFFSET und FETCH in einer Sicht hat keinen Einfluss auf die Updateability-Eigenschaft derselben.  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Die Anzahl der Spalten in der ORDER BY-Klausel ist nicht begrenzt. Die Gesamtgröße der Spalten, die in einer ORDER BY-Klausel angegeben wurden, darf jedoch 8.060 Bytes nicht übersteigen.  
  
 Spalten vom Typ **ntext**, **text**, **image**, **geography**, **geometry** und **xml** können nicht in einer ORDER BY-Klausel verwendet werden.  
  
 Eine Ganzzahl oder Konstante kann nicht angegeben werden, wenn *order_by_expression* in einer Rangfolgefunktion angezeigt wird. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 Wenn Tabellennamen in der FROM-Klausel ein Alias zugeordnet ist, können nur die Aliasnamen verwendet werden, um ihre Spalten in der ORDER BY-Klausel anzugeben.  
  
 Spaltennamen und Aliase, die in der ORDER BY-Klausel angegeben wurden, müssen in der Auswahlliste definiert werden, wenn die SELECT-Anweisung eine der folgenden Klauseln oder Operatoren enthält:  
  
-   UNION-Operator  
  
-   EXCEPT-Operator  
  
-   INTERSECT-Operator  
  
-   SELECT DISTINCT  
  
 Wenn die Anweisung einen UNION-, EXCEPT- oder INTERSECT-Operator enthält, gilt außerdem, dass die Spaltennamen oder Spaltenaliasnamen in der Auswahlliste der ersten (linken) Abfrage angegeben werden müssen.  
  
 In einer Abfrage, die die Operatoren UNION, EXCEPT oder INTERSECT verwendet, wird ORDER BY nur am Ende der Anweisung zugelassen. Diese Einschränkung ist nur gültig, wenn UNION, EXCEPT und INTERSECT in einer Abfrage der obersten Ebene verwendet werden, nicht in einer Unterabfrage. Weitere Informationen finden Sie im Abschnitt "Beispiele" weiter unten.  
  
 Die ORDER BY-Klausel ist in Sichten, Inlinefunktionen, abgeleiteten Tabellen und Unterabfragen nicht gültig, es sei denn, die TOP- oder die OFFSET- und die FETCH-Klausel werden ebenfalls angegeben. Wenn ORDER BY in diesen Objekten verwendet wird, werden mit der Klausel nur die Zeilen bestimmt, die von der TOP-Klausel oder von der OFFSET- und der FETCH-Klausel zurückgegeben werden. Durch die ORDER BY-Klausel wird keine bestimmte Ergebnisreihenfolge bei der Abfrage dieser Konstrukte sichergestellt, es sei denn, in der Abfrage selbst ist ebenfalls ORDER BY angegeben.  
  
 OFFSET und FETCH werden in indizierten Sichten oder einer Sicht, die mit der CHECK OPTION-Klausel definiert wird, nicht unterstützt.  
  
 OFFSET und FETCH können in jeder Abfrage verwendet werden, die TOP und ORDER BY zulässt. Dabei gelten folgenden Einschränkungen:  
  
-   Die OVER-Klausel unterstützt OFFSET und FETCH nicht.  
  
-   OFFSET und FETCH können in Anweisungen INSERT, UPDATE, MERGE, und DELETE nicht direkt angegeben werden, sondern müssen in eine entsprechende Unterabfrage eingeschlossen werden. Beispielsweise können OFFSET und FETCH in der INSERT INTO SELECT-Anweisung in die SELECT-Anweisung eingeschlossen werden.  
  
-   In einer Abfrage, die die Operatoren UNION, EXCEPT oder INTERSECT verwendet, können OFFSET und FETCH nur in die abschließende Abfrage eingeschlossen werden, die die Reihenfolge der Abfrageergebnisse angibt.  
  
-   TOP kann nicht mit OFFSET und FETCH im gleichen Abfrageausdruck (im gleichen Abfragebereich) kombiniert werden.  
  
## <a name="using-offset-and-fetch-to-limit-the-rows-returned"></a>Verwenden von OFFSET und FETCH zum Einschränken der zurückgegebenen Zeilen  
 Es wird empfohlen, die OFFSET-Klausel und die FETCH-Klausel statt der TOP-Klausel zu verwenden, um eine Abfrageauslagerung zu implementieren und die Anzahl der an eine Clientanwendung gesendeten Zeilen einzuschränken.  
  
 Wenn Sie OFFSET und FETCH als Auslagerungslösung verwenden, muss die Abfrage einmal für jede "Seite" der Daten ausgeführt werden, die an die Clientanwendung zurückgegebenen werden. Um beispielsweise die Ergebnisse der Abfrage in Schritten von 10 Zeilen zurückzugeben, müssen Sie die Abfrage einmal ausführen, damit die Zeilen 1 bis 10 zurückgegeben werden. Wenn Sie die Abfrage erneut ausführen, werden die Zeilen 11 bis 20 zurückgegeben usw. Jede Abfrage ist unabhängig und weist keinen Bezug zur anderen auf. Dies bedeutet, dass im Gegensatz zur Verwendung eines Cursors, bei dem die Abfrage einmal ausgeführt und der Status auf dem Server beibehalten wird, die Clientanwendung für das Nachverfolgen des Status zuständig ist. Um stabile Ergebnisse zwischen Abfrageanforderungen mit OFFSET und FETCH zu erreichen, müssen die folgenden Bedingungen erfüllt werden:  
  
1.  Die zugrunde liegenden Daten, die von der Abfrage verwendet werden, dürfen sich nicht ändern. Dies bedeutet, dass die von der Abfrage erfassten Zeilen nicht aktualisiert werden oder alle Anforderungen für Seiten von der Abfrage mit einer Momentaufnahme in einer einzelnen Transaktion oder einer serialisierbare Transaktionsisolationsstufe ausgeführt werden. Weitere Informationen zu Transaktionsisolationsstufen finden Sie unter [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
2.  Die ORDER BY-Klausel enthält eine Spalte oder eine Kombination von Spalten, die garantiert nur einmal vorhanden sind.  
  
 Weitere Informationen finden Sie im Beispiel "Ausführen von mehreren Abfragen in einer einzelnen Transaktion" im Abschnitt "Beispiele" weiter unten in diesem Thema.  
  
 Wenn konsistente Ausführungspläne in der Auslagerungslösung wichtig sind, können Sie den OPTIMIZE FOR-Abfragehinweis für den OFFSET-Parameter und den FETCH-Parameter verwenden. Weitere Informationen finden Sie unter "Angeben von Ausdrücken für OFFSET- und FETCH-Werten" im Abschnitt "Beispiele" weiter unten in diesem Thema. Weitere Informationen zu OPTIMIZE FOR finden Sie unter [Hinweise &#40;Transact-SQL&#41; – Abfrage](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Beispiele  
  
|Category|Funktionssyntaxelemente|  
|--------------|------------------------------|  
|[Grundlegende Syntax](#BasicSyntax)|ORDER BY|  
|[Angeben der auf- und absteigenden Sortierreihenfolge](#SortOrder)|DESC • ASC|  
|[Angeben einer Sortierung](#Collation)|COLLATE|  
|[Angeben einer bedingten Reihenfolge](#Case)|CASE-Ausdruck|  
|[Verwenden von ORDER BY in einer Rangfolgefunktion](#Rank)|Rangfolgefunktionen|  
|[Beschränken der Anzahl der zurückgegebenen Zeilen](#Offset)|OFFSET • FETCH|  
|[Verwenden von ORDER BY mit UNION, EXCEPT und INTERSECT](#Union)|UNION|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Grundlegende Syntax  
 Anhand von Beispielen in diesem Abschnitt wird die grundlegende Funktion der ORDER BY-Klausel mithilfe der mindestens erforderlichen Syntax veranschaulicht.  
  
#### <a name="a-specifying-a-single-column-defined-in-the-select-list"></a>A. Angeben einer einzelnen Spalte, die in der Auswahlliste definiert ist  
 Im folgenden Beispiel wird das Resultset anhand der numerischen `ProductID`-Spalte sortiert. Da keine bestimmte Sortierreihenfolge angegeben wird, wird die Standardsortierung (aufsteigende Reihenfolge) verwendet.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID;  
```  
  
#### <a name="b-specifying-a-column-that-is-not-defined-in-the-select-list"></a>B. Angeben einer Spalte, die nicht in der Auswahlliste definiert wird  
 Im folgenden Beispiel wird das Resultset anhand einer Spalte sortiert, die nicht in der Auswahlliste enthalten ist, jedoch in der Tabelle in der FROM-Klausel definiert wird.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name, Color  
FROM Production.Product  
ORDER BY ListPrice;  
```  
  
#### <a name="c-specifying-an-alias-as-the-sort-column"></a>C. Angeben eines Alias als Sortierspalte  
 Im folgenden Beispiel wird der Spaltenalias `SchemaName` als Sortierspalte angegeben.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT name, SCHEMA_NAME(schema_id) AS SchemaName  
FROM sys.objects  
WHERE type = 'U'  
ORDER BY SchemaName;  
```  
  
#### <a name="d-specifying-an-expression-as-the-sort-column"></a>D: Angeben eines Ausdrucks als Sortierspalte  
 Im folgenden Beispiel wird ein Ausdruck als Sortierspalte verwendet. Der Ausdruck wird mit der DATEPART-Funktion definiert, um das Resultset nach dem Jahr zu sortieren, in dem ein Mitarbeiter eingestellt wurde.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY DATEPART(year, HireDate);  
```  
  
###  <a name="specifying-ascending-and-descending-sort-order"></a><a name="SortOrder"></a> Angeben der auf- und absteigenden Sortierreihenfolge  
  
#### <a name="a-specifying-a-descending-order"></a>A. Angeben einer absteigenden Reihenfolge  
 Im folgenden Beispiel wird das Resultset anhand der numerischen `ProductID`-Spalte in absteigender Reihenfolge sortiert.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID DESC;  
```  
  
#### <a name="b-specifying-an-ascending-order"></a>B. Angeben einer aufsteigenden Reihenfolge  
 Im folgenden Beispiel wird das Resultset anhand der `Name`-Spalte in aufsteigender Reihenfolge sortiert. Die Zeichen sind alphabetisch und nicht numerisch sortiert. Das heißt, 10 steht in der Sortierreihenfolge vor 2.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY Name ASC ;  
```  
  
#### <a name="c-specifying-both-ascending-and-descending-order"></a>C. Angeben von auf- und absteigender Reihenfolge  
 Im folgenden Beispiel wird das Resultset anhand von zwei Spalten sortiert. Das Abfrageresultset wird zunächst anhand der `FirstName`-Spalte in aufsteigender und anschließend anhand der `LastName`-Spalte in absteigender Reihenfolge sortiert.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'R%'  
ORDER BY FirstName ASC, LastName DESC ;  
```  
  
###  <a name="specifying-a-collation"></a><a name="Collation"></a> Angeben einer Sortierung  
 Im folgenden Beispiel wird gezeigt, wie sich die Reihenfolge, in der die Abfrageergebnisse zurückgegeben werden, durch Angeben einer Sortierung in der ORDER BY-Klausel ändern kann. Es wird eine Tabelle mit einer Spalte erstellt, bei deren Sortierung weder die Groß- und Kleinschreibung beachtet noch nach Akzent unterschieden wird. Werte werden mit Groß- und Kleinschreibung sowie unterschiedlichen Akzenten eingefügt. Da in der ORDER BY-Klausel keine Sortierung angegeben wurde, wird von der ersten Abfrage die Sortierung der Spalte beim Sortieren der Werte verwendet. In der zweiten Abfrage wird in der ORDER BY-Klausel eine Sortierung angegeben, bei der die Groß- und Kleinschreibung beachtet und Akzente unterschieden werden; dadurch ändert sich die Reihenfolge, in der die Zeilen zurückgegeben werden.  
  
```sql
USE tempdb;  
GO  
CREATE TABLE #t1 (name NVARCHAR(15) COLLATE Latin1_General_CI_AI)  
GO  
INSERT INTO #t1 VALUES(N'Sánchez'),(N'Sanchez'),(N'sánchez'),(N'sanchez');  
  
-- This query uses the collation specified for the column 'name' for sorting.  
SELECT name  
FROM #t1  
ORDER BY name;  
-- This query uses the collation specified in the ORDER BY clause for sorting.  
SELECT name  
FROM #t1  
ORDER BY name COLLATE Latin1_General_CS_AS;  
```  
  
###  <a name="specifying-a-conditional-order"></a><a name="Case"></a> Angeben einer bedingten Reihenfolge  
 In den folgenden Beispielen wird der CASE-Ausdruck in einer ORDER BY-Klausel verwendet, um die Sortierreihenfolge der Zeilen auf Grundlage eines angegebenen Spaltenwerts bedingt zu bestimmen. Im ersten Beispiel wird der Wert in der `SalariedFlag`-Spalte der `HumanResources.Employee`-Tabelle ausgewertet. Mitarbeiter, deren `SalariedFlag` auf 1 festgelegt wurde, werden nach `BusinessEntityID` in absteigender Folge zurückgegeben. Mitarbeiter, deren `SalariedFlag` auf 0 festgelegt wurde, werden nach `BusinessEntityID` in aufsteigender Folge zurückgegeben. Im zweiten Beispiel wird das Resultset nach der `TerritoryName`-Spalte sortiert, wenn die `CountryRegionName`-Spalte gleich 'United States' ist, und bei allen anderen Zeilen nach `CountryRegionName`.  
  
```sql
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO  
```  
  
```sql
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
###  <a name="using-order-by-in-a-ranking-function"></a><a name="Rank"></a> Verwenden von ORDER BY in einer Rangfolgefunktion  
 Im folgenden Beispiel wird die ORDER BY-Klausel in den Rangfolgefunktionen ROW_NUMBER, RANK, DENSE_RANK und NTILE verwendet.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS "Rank"  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS "Quartile"  
    ,s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
###  <a name="limiting-the-number-of-rows-returned"></a><a name="Offset"></a> Beschränken der Anzahl der zurückgegebenen Zeilen  
 In den folgenden Beispielen wird die Anzahl der Zeilen, die von einer Abfrage zurückgegeben werden, mit OFFSET und FETCH eingeschränkt.  
  
**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
#### <a name="a-specifying-integer-constants-for-offset-and-fetch-values"></a>A. Angeben von ganzzahligen Konstanten für OFFSET- und FETCH-Werte  
 Im folgenden Beispiel wird eine ganzzahlige Konstante als Wert für die OFFSET- und die FETCH-Klausel angegeben. Die erste Abfrage gibt alle Zeilen nach der `DepartmentID`-Spalte sortiert zurück. Vergleichen Sie die von dieser Abfrage zurückgegebenen Ergebnisse mit denen der beiden folgenden Abfragen. In der folgenden Abfrage werden mit der `OFFSET 5 ROWS`-Klausel die ersten 5 Zeilen übersprungen und alle verbleibenden Zeilen zurückgegeben. In der letzten Abfrage wird mit der `OFFSET 0 ROWS`-Klausel bei der ersten Zeile begonnen, und anschließend wird mit `FETCH NEXT 10 ROWS ONLY` die Anzahl der zurückgegebenen Zeilen vom sortierten Resultset auf 10 begrenzt.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Return all rows sorted by the column DepartmentID.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID;  
  
-- Skip the first 5 rows from the sorted result set and return all remaining rows.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID OFFSET 5 ROWS;  
  
-- Skip 0 rows and return only the first 10 rows from the sorted result set.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID   
    OFFSET 0 ROWS  
    FETCH NEXT 10 ROWS ONLY;  
```  
  
#### <a name="b-specifying-variables-for-offset-and-fetch-values"></a>B. Angeben von Variablen für OFFSET- und FETCH-Werte  
 Im folgenden Beispiel werden die Variablen `@RowsToSkip` und `@FetchRows` deklariert und in der OFFSET- und in der FETCH-Klausel angegeben.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Specifying variables for OFFSET and FETCH values    
DECLARE @RowsToSkip TINYINT = 2
      , @FetchRows TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @RowsToSkip ROWS   
    FETCH NEXT @FetchRows ROWS ONLY;  
```  
  
#### <a name="c-specifying-expressions-for-offset-and-fetch-values"></a>C. Angeben von Ausdrücken für OFFSET- und FETCH-Werte  
 Im folgenden Beispiel wird der OFFSET-Wert mit dem Ausdruck `@StartingRowNumber - 1` und der FETCH-Wert mit dem Ausdruck `@EndingRowNumber - @StartingRowNumber + 1` angegeben. Außerdem wird der Abfragehinweis OPTIMIZE FOR angegeben. Mit dem Abfragehinweis kann ein bestimmter Wert für eine lokale Variable bereitgestellt werden, wenn die Abfrage kompiliert und optimiert wird. Dieser Wert wird nur während der Abfrageoptimierung verwendet, nicht während der Abfrageausführung. Weitere Informationen finden Sie unter [Abfragehinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Specifying expressions for OFFSET and FETCH values      
DECLARE @StartingRowNumber TINYINT = 1  
      , @EndingRowNumber TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @EndingRowNumber - @StartingRowNumber + 1 ROWS ONLY  
OPTION ( OPTIMIZE FOR (@StartingRowNumber = 1, @EndingRowNumber = 20) );  
```  
  
#### <a name="d-specifying-a-constant-scalar-subquery-for-offset-and-fetch-values"></a>D: Angeben einer konstanten skalaren Unterabfrage für OFFSET- und FETCH-Werte  
 Im folgenden Beispiel wird der Wert für die FETCH-Klausel mit einer konstanten skalaren Unterabfrage definiert. Die Unterabfrage gibt einen einzelnen Wert von der Spalte `PageSize` in der Tabelle `dbo.AppSettings` zurück.  
  
```sql
-- Specifying a constant scalar subquery  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.AppSettings (AppSettingID INT NOT NULL, PageSize INT NOT NULL);  
GO  
INSERT INTO dbo.AppSettings VALUES(1, 10);  
GO  
DECLARE @StartingRowNumber TINYINT = 1;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber ROWS   
    FETCH NEXT (SELECT PageSize FROM dbo.AppSettings WHERE AppSettingID = 1) ROWS ONLY;  
```  
  
#### <a name="e-running-multiple-queries-in-a-single-transaction"></a>E. Ausführen mehrerer Abfragen in einer einzelnen Transaktion  
 Im folgenden Beispiel wird eine Methode veranschaulicht, eine Auslagerungslösung zu implementieren, die sicherstellt, dass in allen Anforderungen beständige Ergebnisse von der Abfrage zurückgegeben werden. Die Abfrage wird mit der Momentaufnahmeisolationsstufe in einer einzelnen Transaktion ausgeführt, und die in der ORDER BY-Klausel angegebene Spalte stellt die Eindeutigkeit der Spalten sicher.  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Ensure the database can support the snapshot isolation level set for the query.  
IF (SELECT snapshot_isolation_state FROM sys.databases WHERE name = N'AdventureWorks2012') = 0  
    ALTER DATABASE AdventureWorks2012 SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Set the transaction isolation level  to SNAPSHOT for this query.  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
-- Beginning the transaction.
BEGIN TRANSACTION;  
GO  
-- Declare and set the variables for the OFFSET and FETCH values.  
DECLARE @StartingRowNumber INT = 1  
      , @RowCountPerPage INT = 3;  
  
-- Create the condition to stop the transaction after all rows have been returned.  
WHILE (SELECT COUNT(*) FROM HumanResources.Department) >= @StartingRowNumber  
BEGIN  
  
-- Run the query until the stop condition is met.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @RowCountPerPage ROWS ONLY;  
  
-- Increment @StartingRowNumber value.  
SET @StartingRowNumber = @StartingRowNumber + @RowCountPerPage;  
CONTINUE  
END;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
###  <a name="using-order-by-with-union-except-and-intersect"></a><a name="Union"></a> Verwenden von ORDER BY mit UNION, EXCEPT und INTERSECT  
 Wenn eine Abfrage die Operatoren UNION, EXCEPT oder INTERSECT verwendet, muss die ORDER BY-Klausel am Ende der Anweisung angegeben werden, und die Ergebnissen der kombinierten Abfragen werden sortiert. Im folgenden Beispiel werden alle Produkte zurückgegeben, die rot oder gelb sind, und die kombinierte Liste wird anhand der Spalte `ListPrice` sortiert.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Red'  
-- ORDER BY cannot be specified here.  
UNION ALL  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Yellow'  
ORDER BY ListPrice ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Das folgende Beispiel veranschaulicht ein Resultset sortiert nach der numerischen `EmployeeKey`-Spalte in aufsteigender Reihenfolge.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey;  
```  
  
 Im folgenden Beispiel wird ein Resultset anhand der numerischen `EmployeeKey`-Spalte in absteigender Reihenfolge sortiert.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey DESC;  
```  
  
 Im folgenden Beispiel wird das Resultset anhand der `LastName`-Spalte sortiert.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName;  
```  
  
 Im folgenden Beispiel wird das Resultset anhand von zwei Spalten sortiert. In dieser Abfrage wird zuerst nach der `FirstName`-Spalte in aufsteigender Reihenfolge sortiert, und anschließend werden `FirstName`-Werte nach der `LastName`-Spalte in absteigender Reihenfolge sortiert.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName, FirstName;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Rangfolgefunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)   
 [Abfragehinweise (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)   
 [EXCEPT und INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  

